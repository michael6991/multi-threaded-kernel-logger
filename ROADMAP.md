🚀 Roadmap for Multi-threaded Logger with Kernel Module

## Phase 1 – User-Space Only (Threads + Mutexes)
Goal: Build the producer/consumer system entirely in user-space.
* Step 1: Create a bounded queue (circular buffer) for log messages.
* Step 2: Spawn multiple producer threads that generate random log messages (e.g., "Producer 1: message 42").
* Step 3: Spawn one consumer thread that dequeues and prints the messages.
* Step 4: Use pthread_mutex_t + pthread_cond_t to synchronize access to the queue (classic producer-consumer problem).
👉 At this point, you’ve practiced POSIX mutual exclusion + multi-threading.

## Phase 2 – Kernel-Space Sink (Character Device Driver)
Goal: Replace the user-space consumer with a kernel module.
* Step 1: Write a minimal Linux kernel module that registers a character device /dev/logger.
* Step 2: Implement open(), release(), and a basic write() that stores data into a kernel buffer (kmalloc + ring buffer).
* Step 3: From user-space, replace the consumer thread with write() system calls into /dev/logger.
👉 Now you’re touching user ↔ kernel interaction.

## Phase 3 – Reading Back Logs
Goal: Make logs retrievable.
* Step 1: Implement a read() method in your driver that lets another user-space program fetch logs.
* Step 2: Write a separate consumer app (or thread) that calls read() on /dev/logger and prints messages.
👉 You now have a multi-threaded user-space producer → kernel buffer → user-space consumer pipeline.

## Phase 4 – Advanced Features (Optional)
If you have energy to push further:
1. IOCTL commands
* clear_buffer, get_stats (bytes written, number of messages, etc).
* Practice unlocked_ioctl in kernel.
2. Blocking Reads
* If buffer is empty, read() blocks until new logs arrive.
* Practice wait queues in kernel.
3. poll/select/epoll support
* Allow user-space to wait for data availability.
* Multi-consumer Support
* Readers get separate views of the buffer (like dmesg).

📌 Suggested Order of Work
1. Producer/consumer in user-space only (safe playground).
2. Kernel char device with write() support.
3. Integrate user-space producers to write logs to kernel.
4. Add read() support and consumer app.
5. (Optional) Add ioctl, blocking I/O, poll/select.

👉 By the end, you’ll have touched:
* POSIX threading & mutexes (Phase 1).
* Kernel module & character device APIs (Phase 2+).
* User ↔ kernel communication (syscalls, ioctl, poll) (Phase 3+).
