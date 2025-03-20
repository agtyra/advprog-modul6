# Module 6 - Concurrency
 Kezia Salsalina Agtyra Sebayang (2306172086)
 
## Commit 1: Reflection Notes

 The handle_connection function reads and processes an incoming TCP stream in a Rust web server. It wraps the stream in a BufReader to read data efficiently line by line. The function collects HTTP request lines into a vector, stopping at the first empty line, which marks the end of the headers. Finally, it prints the captured request to the console. While this approach effectively reads HTTP requests, it has some limitations. Using .unwrap() can cause the server to panic if an error occurs while reading the stream. 

## Commit 2: Reflection Notes

![Commit 2 screen capture](/assets/images/commit2.png)

 This NEW function is an improved version of the previous one. Before, it only read and printed the incoming HTTP request, but now it actually responds to the client. It still reads the request line by line using BufReader, stopping at the first empty line (which marks the end of the headers). But after that, instead of just printing the request, it builds an HTTP response. It sets the status line to "HTTP/1.1 200 OK", reads the contents of hello.html, and calculates its length. Then, it formats the response, including the content length header, and writes it back to the client.

## Commit 3: Reflection Notes

![Commit 3 screen capture](/assets/images/commit3.png)

This version of handle_connection improves the previous one by introducing request handling logic. Instead of blindly responding with hello.html, it now checks the request line to determine the appropriate response. If the request is "GET / HTTP/1.1", it serves hello.html with a 200 OK status. Otherwise, it returns 404.html with a 404 NOT FOUND status. The key change here is separating request handling from response construction. Previously, the function always sent the same response regardless of what the client requested. Now, it dynamically selects the status code and file based on the request, making it more adaptable.

## Commit 4: Reflection Notes

This version expands the server’s functionality by introducing a /sleep route that delays the response by 10 seconds before sending back hello.html. When testing with two browser windows, if one accesses /sleep, the other request (to /) also experiences a delay. This happens because the server handles requests sequentially, meaning each request blocks the next one until it’s completed. If multiple users were to access /sleep simultaneously, the server would struggle to respond quickly, causing noticeable slowdowns. This highlights a key limitation: the server is not designed to handle multiple connections at once. In a real-world scenario, a slow request like this could significantly degrade performance for all users.

## Commit 5: Reflection Notes

This version introduces a ThreadPool, allowing the server to handle multiple requests concurrently instead of sequentially. Previously, a slow request like /sleep would block all others, but now, multiple worker threads can handle different requests at the same time.
The ThreadPool works by maintaining a fixed number of worker threads that listen for incoming jobs. When the server receives a new request, it sends the job (a closure) to a worker via a message-passing channel. Each worker continuously waits for a job, locks the receiver, retrieves a task, and executes it. By using Arc<Mutex<T>>, multiple workers can safely share the receiver while ensuring only one thread processes a job at a time. This prevents race conditions while allowing concurrency. Now, if multiple users request /sleep, the server can still respond to other requests without stalling.
This implementation makes the server more efficient and scalable, as it can now handle multiple requests in parallel instead of being blocked by long-running tasks.

## Bonus : Reflection Notes

I modified the ThreadPool implementation by replacing new with build as the constructor. Functionally, nothing really changes—build still sets up the worker threads, message-passing channel, and ensures safe access using Arc<Mutex<T>>. But the name build makes it clearer that this function is assembling something more complex rather than just creating a basic instance.

Rust typically uses new for constructors, so at first, it might seem unnecessary. But in this case, build makes more sense because setting up a thread pool isn’t just a simple new call—it involves spawning multiple threads and preparing them to handle jobs. This small change improves readability and makes it more obvious that the function is doing more than just returning an object. It also leaves room for adding different ways to initialize the pool later if needed.