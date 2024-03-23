# Module 6 - Aliyah Faza Qinthara

### `handle_connection` Method
- `handle_connection(mut stream: TcpStream)`: This is the function definition for `handle_connection`. It takes a mutable `TcpStream` as an argument. `TcpStream` is a struct provided by Rust’s standard library that represents a TCP connection from a client.
- `let buf_reader = BufReader::new(&mut stream);`: This line creates a new instance of `BufReader` (a buffered reader) from the mutable reference to the `stream`. `BufReader` is used to add buffering to `Read` instances.
- `let http_request: Vec<_> = buf_reader.lines()...`: This line is creating a `Vec` (vector) named `http_request`. The `_` in `Vec<_>` is a type placeholder, meaning the compiler will infer the type based on the values that get pushed into the vector.
- `.lines()`: This is a method call on `buf_reader` that returns an iterator over the lines of the reader.
- `.map(|result| result.unwrap())`: This is a method call on the iterator that applies a function to each item in the iterator. In this case, the function is a closure that takes a `result` and calls the `unwrap` method on it. `unwrap` will return the value inside an `Ok` variant of a `Result` or panic if it’s an `Err`.
- `.take_while(|line| !line.is_empty())`: This is another method call on the iterator that takes elements from the iterator while a predicate is true. Here, it’s taking lines while they are not empty.
- `.collect()`: This is a method call on the iterator that consumes the iterator and collects the values into a collection. In this case, it’s collecting the values into the `http_request` vector.
- `println!("Request: {:#?}", http_request);`: This line prints the `http_request` vector to the console. The `:#?` inside the string is a debug format specifier used for pretty-printing `Debug` output.

### `handle_connection` Method 2.0
The modified `handle_connection` function in Rust continues to read an incoming HTTP request from a TCP stream. However, it now also constructs an HTTP response. This is achieved by reading the contents of a file named "hello.html", calculating its length, and formatting an HTTP response string with a status line, content length header, and the file contents as the body. This response is then written back to the TCP stream.

My screen:
![My Hello Screen](/assets/images/commit2.png)
