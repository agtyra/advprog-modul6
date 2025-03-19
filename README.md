# Module 6 - Concurrency
 **Kezia Salsalina Agtyra Sebayang (2306172086)**
 
## Commit 1: Reflection Notes

 The handle_connection function reads and processes an incoming TCP stream in a Rust web server. It wraps the stream in a BufReader to read data efficiently line by line. The function collects HTTP request lines into a vector, stopping at the first empty line, which marks the end of the headers. Finally, it prints the captured request to the console.

## Commit 2: Reflection Notes

![Commit 2 screen capture](/assets/images/commit2.png)

 This NEW function is an improved version of the previous one. Before, it only read and printed the incoming HTTP request, but now it actually responds to the client. It still reads the request line by line using BufReader, stopping at the first empty line (which marks the end of the headers). But after that, instead of just printing the request, it builds an HTTP response. It sets the status line to "HTTP/1.1 200 OK", reads the contents of hello.html, and calculates its length. Then, it formats the response, including the content length header, and writes it back to the client.

## Commit 3: Reflection Notes

![Commit 3 screen capture](/assets/images/commit3.png)

This version of handle_connection improves the previous one by introducing request handling logic. Instead of blindly responding with hello.html, it now checks the request line to determine the appropriate response. If the request is "GET / HTTP/1.1", it serves hello.html with a 200 OK status. Otherwise, it returns 404.html with a 404 NOT FOUND status. The key change here is separating request handling from response construction. Previously, the function always sent the same response regardless of what the client requested. Now, it dynamically selects the status code and file based on the request, making it more adaptable.
