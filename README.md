# Commit 1: Reflection notes
The Rust code snippets is a part of a server application that reads an HTTP request from the client. 
The `BufReader::new(&mut stream)` function call creates a new instance of `BufReader` - a buffered reader. This is a type of reader that reads from a buffer, which can be more efficient than reading directly from a source like a network stream. The `&mut stream` argument is a mutable reference to the stream from which the `BufReader` will read.

The `lines()` method is then called on the `BufReader` instance. This method returns an iterator over the lines of the stream. Each item in the iterator is a `Result<String, Error>`, where `String` is the line read from the stream and Error is any error that occurred while reading the line.

The `map(|result| result.unwrap())` function call uses the `map` method to transform the `Result<String, Error>` items into `String` items. The `unwrap()` method is called on each `Result`, which returns the `String` if the` Result` is Ok, or panics if the `Result` is `Err`.

The `take_while(|line| !line.is_empty())` function call uses the `take_while` method to take lines from the iterator as long as the provided predicate function returns true. The predicate function `|line| !line.is_empty()` returns `true` as long as the line is not empty.

The `collect()` method is then called on the iterator, which transforms the iterator into a collection. In this case, the collection is a `Vec<String>`, which is a vector of strings.

Finally, the `println!("Request: {:#?}", http_request);` line prints the HTTP request to the console. The `{:#?}` is a placeholder for the `http_request` variable, and the `#` option tells Rust to use "pretty printing", which formats the output in a more readable way.

