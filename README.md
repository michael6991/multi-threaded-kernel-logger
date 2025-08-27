# Project: Multi-threaded Logger with a Kernel Module Back-end
## High-Level Description

Implement a user-space program that spawns multiple threads (producers) which generate log messages. These threads push logs into a shared buffer protected by POSIX mutexes/condition variables.

Instead of writing directly to a file, the program sends the logs to a character device implemented as a Linux kernel module. The kernel module acts as the log "sink" and writes the messages into the kernel buffer. A user-space consumer program can later read logs back from this device.

### What Skills You’ll Practice
* Linux user-space programming
* Multi-threaded producer threads.
* Synchronization using pthread_mutex_t, pthread_cond_t.
* Handling multiple readers/writers.

### User ↔ Kernel interaction
* Create a simple Linux character device driver (/dev/logger).
* Support write() from user-space (log producers send logs).
* Support read() from user-space (log consumers fetch logs).
* Use ioctl() for extra functionality (e.g., clear logs, get stats).

### POSIX mutual exclusion & threading
* Shared buffer management in user-space with mutexes.
* Synchronization of threads writing to the kernel device.
* Possibly add a read-write lock if multiple consumers are allowed.

## How It Ties Together
1. User-space threads (producers): Generate messages concurrently.
2. POSIX synchronization: Protect a bounded queue of log messages before writing them to the device.
3. System calls: Use open(), write(), read(), and optionally ioctl() to interact with the kernel.
4. Kernel module (character device): Acts as the log storage in kernel-space.

## Possible Extensions (if you want to push further)
* Add blocking/non-blocking I/O support in the driver.
* Use poll/select/epoll from user-space to wait for data readiness.
* Implement priority logs (kernel sorts messages).
* Add shared memory mapping (mmap) instead of read/write for faster transfers.

✅ This project is not too big (fits in a few hundred lines split between kernel + user-space), but it will give you hands-on experience with Linux multi-threading, POSIX mutexes, and kernel interaction.
