# Day 3: Node.js Internals (V8, libuv, Threads)

## 1. What is Node.js?
Node.js is **not** a programming language or a framework. It is a **JavaScript Runtime Environment** built on Chrome's V8 JavaScript engine. It allows JavaScript to be executed on the server side.

### The Architecture Stack:
1.  **JavaScript Layer**: Your code and Node.js core modules (fs, http, path).
2.  **C++ Bindings**: The "bridge" that allows JS to communicate with the C++ backend.
3.  **V8 Engine**: Converts JS code into machine code.
4.  **libuv**: Handles asynchronous I/O, the Event Loop, and the Thread Pool.

---

## 2. The V8 Engine (The Brain)
V8 is Google’s open-source high-performance JavaScript and WebAssembly engine, written in C++.

### How V8 Works:
*   **Parser**: Converts JS code into an **Abstract Syntax Tree (AST)**.
*   **Ignition (Interpreter)**: V8 doesn't just "read" JS; it compiles it to bytecode. Ignition is the interpreter that generates this bytecode.
*   **TurboFan (JIT Compiler)**: V8 tracks "hot code" (code run frequently). TurboFan takes that bytecode and compiles it into highly optimized **Machine Code**.
*   **Garbage Collection (Orinoco)**: Automatically reclaims memory. It uses a **Mark-and-Sweep** algorithm and is generational (Young vs. Old generation).

---

## 3. libuv (The Heart)
libuv is a multi-platform C library that provides support for asynchronous I/O based on event loops. It was originally written for Node.js.

### Key Responsibilities:
1.  **Event Loop**: The mechanism that coordinates the execution of callbacks.
2.  **Thread Pool**: By default, libuv has a pool of **4 threads** (can be increased up to 1024 via `UV_THREADPOOL_SIZE`). It handles heavy tasks that the OS doesn't provide async APIs for (like file I/O, DNS lookups, and crypto).
3.  **Async I/O**: Handles networking, timers, and child processes.

---

## 4. Single-Threaded vs. Multi-Threaded
Node.js is often called "single-threaded," but that is a half-truth.

*   **The Main Thread**: Node.js has **one** main thread where the Event Loop runs. All your JavaScript code executes here.
*   **The Thread Pool (Worker Pool)**: For heavy lifting (File System, Crypto, Zlib), Node.js offloads work to the libuv thread pool.
*   **OS Level**: For things like Networking (TCP/UDP), Node.js uses the OS's own asynchronous primitives (epoll on Linux, Kqueue on macOS, IOCP on Windows). This does **not** use the thread pool.

### Why not use more threads for everything?
Context switching between threads is expensive in terms of CPU and memory. For I/O bound tasks, an event-driven non-blocking model is much more efficient than spawning a thread for every request.

---

## 5. Worker Threads (Since Node 10.5.0)
If you have **CPU-intensive tasks** (like image processing or heavy math), you can use the `worker_threads` module. This allows you to run JavaScript in parallel on multiple threads, sharing memory via `ArrayBuffer` or `SharedArrayBuffer`.

---

## 6. Summary of Execution Flow
1.  V8 compiles your JS code.
2.  If the code is synchronous, it runs on the main thread.
3.  If it's asynchronous (e.g., `fs.readFile`):
    *   Node offloads it to **libuv**.
    *   Libuv checks if the OS can handle it (Networking) or if it needs the **Thread Pool** (File I/O).
    *   The Main Thread continues running other code (Non-blocking).
    *   Once the task is done, the callback is pushed to the **Callback Queue**.
    *   The **Event Loop** picks up the callback and runs it on the main thread.
