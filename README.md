# Module 6 - Concurrency
 **Kezia Salsalina Agtyra Sebayang (2306172086)**
 
 ## Commit 1: Reflection Notes

 The handle_connection function reads and processes an incoming TCP stream in a Rust web server. It wraps the stream in a BufReader to read data efficiently line by line. The function collects HTTP request lines into a vector, stopping at the first empty line, which marks the end of the headers. Finally, it prints the captured request to the console.