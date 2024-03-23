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
![My Hello Screen](https://cdn.discordapp.com/attachments/1030834426126544907/1221050530848313354/Screenshot_2024-03-23_171351.png?ex=66112aec&is=65feb5ec&hm=b213888916de2aebadf64516cc752ccbd56ff54a96012e83ca2d8e66b18bbd60&)

### Validating Request and Refactoring
We pull out those differences into separate `if` and `else` lines that will assign the values of the status line and the filename to variables; we can then use those variables unconditionally in the code to read the file and write the response. The previously duplicated code is now outside the `if` and `else` blocks and uses the `status_line` and `filename` variables. This makes it easier to see the difference between the two cases, and it means we have only one place to update the code if we want to change how the file reading and response writing work.

My screen:
![My Oops Screen](https://cdn.discordapp.com/attachments/1030834426126544907/1221050506026156154/Screenshot_2024-03-23_174808.png?ex=66112ae6&is=65feb5e6&hm=c85071f13a93e692e0da3bb991126569d31eeb88cbbe26586ea6e189be317908&)

### Sleep
Our second arm in `handle_connection` method matches a request to _/sleep_. When that request is received, the server will sleep for 5 seconds before rendering the successful HTML page.

### Multithreaded Server
A thread pool efficiently manages multiple tasks by initializing a fixed number of worker threads, each waiting for tasks in a loop. Tasks are submitted to the pool and stored in a queue, with worker threads picking up tasks as they become available. Concurrency is maintained by limiting the number of threads and queuing tasks if all threads are busy. Tasks are executed concurrently, maximizing throughput while preventing resource exhaustion. Safe resource sharing is ensured through mechanisms like '**Arc**' and '**Mutex**'. Overall, a thread pool balances concurrency, resource utilization, and efficient task execution in concurrent programming scenarios, providing a scalable solution for managing asynchronous tasks.
