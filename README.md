# Commit 1: Reflection notes
The Rust code snippets is a part of a server application that reads an HTTP request from the client. 
The `BufReader::new(&mut stream)` function call creates a new instance of `BufReader` - a buffered reader. This is a type of reader that reads from a buffer, which can be more efficient than reading directly from a source like a network stream. The `&mut stream` argument is a mutable reference to the stream from which the `BufReader` will read.

The `lines()` method is then called on the `BufReader` instance. This method returns an iterator over the lines of the stream. Each item in the iterator is a `Result<String, Error>`, where `String` is the line read from the stream and Error is any error that occurred while reading the line.

The `map(|result| result.unwrap())` function call uses the `map` method to transform the `Result<String, Error>` items into `String` items. The `unwrap()` method is called on each `Result`, which returns the `String` if the` Result` is Ok, or panics if the `Result` is `Err`.

The `take_while(|line| !line.is_empty())` function call uses the `take_while` method to take lines from the iterator as long as the provided predicate function returns true. The predicate function `|line| !line.is_empty()` returns `true` as long as the line is not empty.

The `collect()` method is then called on the iterator, which transforms the iterator into a collection. In this case, the collection is a `Vec<String>`, which is a vector of strings.

Finally, the `println!("Request: {:#?}", http_request);` line prints the HTTP request to the console. The `{:#?}` is a placeholder for the `http_request` variable, and the `#` option tells Rust to use "pretty printing", which formats the output in a more readable way.

# Commit 2: Reflection notes
![Commit 2 screen capture](/assets/commit2.png)
This Rust code snippet is part of a server application that sends an HTTP response to a client.

The `let status_line = "HTTP/1.1 200 OK";` line defines a string variable `status_line` that represents the status line of the HTTP response. The status line is the first line of the response and includes the HTTP version (`HTTP/1.1`) and the status code (`200 OK`), which indicates a successful HTTP request.

The `let contents = fs::read_to_string("hello.html").unwrap();` line reads the contents of the file `hello.html`into a string. The `fs::read_to_string` function reads a file into a string and returns a `Result<String, Error>`. The `unwrap()` method is then called on the `Result`, which returns the `String` if the `Result` is `Ok`, or panics if the `Result` is `Err`.

The `let length = contents.len();` line gets the length of the `contents` string, which represents the size of the `hello.html` file in bytes.

The `let response = format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");` line formats the HTTP response. The `format!` macro returns a string formatted according to the format string and arguments. The format string includes placeholders `{}` for the `status_line`, `length`, and `contents` variables. The `\r\n` sequences are carriage return and line feed characters, which are used to separate lines in HTTP headers.

Finally, the `stream.write_all(response.as_bytes()).unwrap();` line sends the HTTP response to the client. The `write_all` method writes all bytes from the `response` string to the stream, and the `as_bytes` method converts the `response` string into bytes. The `unwrap()` method is then called on the `Result` returned by `write_all`, which ensures that the write operation was successful, or panics if an error occurred.

# Commit 3: Reflection notes
![Commit 2 screen capture](/assets/commit3.png)
## How to split between response? 
In the code, I'm splitting the HTTP response into headers and body using the `split` method and the delimiter `\r\n\r\n`. This is a common way to separate the headers from the body in an HTTP message, as per the HTTP protocol.

Here's how it works:

```rust
let response_parts: Vec<&str> = response.split("\r\n\r\n").collect();
```

The `split` method returns an iterator over the substrings of `response` that are separated by the delimiter `\r\n\r\n`. The `collect` method then transforms this iterator into a `Vec<&str>`, which is a vector of string slices.

After this line, `response_parts` is a vector where the first element (`response_parts[0]`) is the headers and the second element (`response_parts[1]`) is the body.

```rust
let headers = response_parts[0];
let body = response_parts[1];
```

These lines simply assign the headers and body to their respective variables.

Finally, you write the headers and body to the stream:

```rust
stream.write_all(headers.as_bytes()).unwrap();
stream.write_all(body.as_bytes()).unwrap();
```

The `write_all` method writes all bytes from the string slice to the stream, and the `as_bytes` method converts the string slice into bytes. The `unwrap` method is then called on the `Result` returned by `write_all`, which ensures that the write operation was successful, or panics if an error occurred.

## Why Refactoring is needed? 
Before refactoring (based on the tutorial book), there's significant redundancy in the if and else blocks: both are involved in file reading and writing operations. The sole distinctions lie in the status line and filename. To enhance code conciseness, we can extract these variances into separate lines within the if and else statements, assigning their values to variables. Subsequently, we can use these variables universally in the code to manage file reading and response writing.

# Commit 4: Reflection 
The `/sleep` url have: 
```rust
        thread::sleep(Duration::from_secs(10)); ("HTTP/1.1 200 OK", "hello.html")
```
inside the code, hence the response time of the html page is longer. The sleep functio from Rust's `std::thread` module pauses the execution of the current thread for a specified duration, in this case the duration is 10 seconds. This is used to simulate the network latency. 

# Commit 5: Reflection
In Rust, a thread pool is a collection of worker threads that are pre-allocated and managed for executing tasks concurrently. Thread pools are commonly used to efficiently handle asynchronous tasks, such as parallelizing CPU-bound work or performing I/O operations without blocking the main thread.