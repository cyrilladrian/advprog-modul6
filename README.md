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
