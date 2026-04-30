# Day 3: Interview Questions & Answers - Node.js Internals

### 1. Is Node.js truly single-threaded?
**Answer:**
Node.js is **single-threaded for JavaScript execution** but **multi-threaded for system tasks**. 
Your JavaScript code runs on a single thread called the **Main Thread**, which hosts the **Event Loop**. However, for tasks like file I/O, cryptography, or compression, Node.js uses **libuv's Thread Pool** (defaulting to 4 threads). For networking tasks, it utilizes the OS kernel's non-blocking mechanisms (like `epoll` or `IOCP`). So, while your code doesn't run in parallel by default, the runtime does.

---

### 2. What is the role of the V8 Engine in Node.js?
**Answer:**
V8 is the engine that executes JavaScript. Its primary roles are:
1.  **Compilation**: It uses a JIT (Just-In-Time) compiler. It interprets code using **Ignition** and optimizes "hot code" using **TurboFan** into machine code.
2.  **Memory Management**: It manages the Heap memory and handles **Garbage Collection**.
3.  **Call Stack**: It maintains the execution stack of JS functions.
Node.js "wraps" V8 and provides APIs (like `fs` or `http`) that V8 doesn't have natively.

---

### 3. What is libuv and why is it important?
**Answer:**
libuv is a C++ library that handles the "asynchronous" part of Node.js. It provides:
1.  **The Event Loop**: The core mechanism that schedules callbacks.
2.  **The Thread Pool**: A set of threads used to handle blocking tasks like file I/O or DNS lookups.
3.  **Cross-platform Abstraction**: It provides a consistent API for I/O across Windows, Linux, and macOS.
Without libuv, Node.js would be blocking and wouldn't be able to handle thousands of concurrent connections efficiently.

---

### 4. When should you increase `UV_THREADPOOL_SIZE`?
**Answer:**
You should increase it when your application performs a high volume of blocking I/O tasks that use the thread pool, such as:
- Heavy file system operations (`fs`).
- Intensive cryptography (`crypto.pbkdf2`, `bcrypt`).
- Heavy compression (`zlib`).
If the pool is saturated (e.g., all 4 threads are busy), new I/O requests will wait in a queue, causing latency. Increasing the size (up to 1024) can help, though it adds some overhead.

---

### 5. How does Node.js handle thousands of concurrent connections if it's single-threaded?
**Answer:**
It uses **Non-blocking I/O and Event Demultiplexing**. 
When a request comes in (e.g., a TCP connection), Node.js doesn't create a new thread. Instead, it registers the connection with the OS (using `epoll` or `kqueue`) and provides a callback. The OS notifies Node.js when data is ready. Since the main thread only handles the "logic" and delegating tasks, it stays free to accept new connections while others are waiting for data.

---

### 6. What are Worker Threads and when would you use them?
**Answer:**
The `worker_threads` module allows you to create multiple instances of the V8 engine running in parallel. 
- **Use Case**: CPU-intensive tasks like video encoding, complex mathematical calculations, or data processing.
- **Why not use them for I/O?**: For I/O, the standard async model is already more efficient. Worker threads are specifically for when the **Main Thread is blocked by heavy computation**, which would otherwise make the entire server unresponsive.
