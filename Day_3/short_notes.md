# Day 3: Node.js Internals - Quick Revision ⚡

### 🧠 V8 Engine
- **Ignition**: The Interpreter (generates bytecode).
- **TurboFan**: The JIT Compiler (optimizes hot code to machine code).
- **Orinoco**: The Garbage Collector.
- **AST**: Abstract Syntax Tree (what the parser builds).

### ❤️ libuv
- Handles **Async I/O**, **Event Loop**, and **Thread Pool**.
- Default Thread Pool Size: **4**.
- Handles: `fs`, `crypto`, `zlib`, `dns.lookup`.

### 🧵 Threads in Node
1.  **Main Thread**: Where the Event Loop and your JS code live.
2.  **Thread Pool**: For heavy I/O (File system, DNS lookup).
3.  **OS Kernel**: For networking (Non-blocking, very efficient).
4.  **Worker Threads**: For CPU-bound tasks (Parallel JS execution).

### 🚀 Key Concepts
- **Non-blocking I/O**: Don't wait for the disk; move to the next task.
- **Single-Threaded**: Only one stack, one heap, and one event loop for JS execution.
- **Context Switching**: Avoided by using an event-driven model instead of many threads.

### ⚙️ Useful Environment Variables
- `UV_THREADPOOL_SIZE=8`: Increases the libuv thread pool.
- `NODE_OPTIONS="--max-old-space-size=4096"`: Increases memory limit for V8.
