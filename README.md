# Concurrency Lab: Channels in C

## Introduction

This repository contains the implementation of a **channel** system in C for synchronization and communication via message passing. Channels act as queues/buffers for messages with a fixed maximum size, enabling multiple senders and receivers to interact concurrently. Both **blocking** and **non-blocking** send/receive operations are supported. This project emphasizes correctness and thread-safe implementation.

---

## Features

### Channel Functions

- **Blocking Operations**:
  - `channel_send`: Blocks the sender if the buffer is full until space becomes available.
  - `channel_receive`: Blocks the receiver if the buffer is empty until data is available.

- **Non-Blocking Operations**:
  - `channel_non_blocking_send`: Returns immediately if the buffer is full.
  - `channel_non_blocking_receive`: Returns immediately if the buffer is empty.

- **Lifecycle Management**:
  - `channel_create`: Initializes a channel with a fixed buffer size.
  - `channel_close`: Closes the channel, disallowing further sends while allowing pending data to be received.
  - `channel_destroy`: Frees memory allocated for the channel.

- **Multiplexing**:
  - `channel_select`: Allows waiting on multiple channels simultaneously to receive data from the first available channel.

---

## Project Structure

### Files

- **Core Files**:
  - `channel.c` and `channel.h`: Main implementation of the channel functionality.
  - `linked_list.c` and `linked_list.h`: Optional linked list utilities.
  - `buffer.c` and `buffer.h`: Buffer management utilities.

- **Test Files**:
  - `test.c`: Contains test cases to validate functionality.
  - `grade.py`: Script for automated grading.

- **Configuration Files**:
  - `Makefile`: Build and testing commands.
  - `Vagrantfile`: Configuration for virtual environment setup.

---

## Building and Testing

### Build Commands

- **Build the Project**:
  ```bash
  make
  ```

- **Build in Debug Mode**:
  ```bash
  make debug
  ```

### Running Tests

- **Run All Tests**:
  ```bash
  make test
  ```

- **Run Specific Test**:
  ```bash
  ./channel <test_case_name> <iterations>
  ```

- **Run with Sanitizer**:
  ```bash
  ./channel_sanitize <test_case_name> <iterations>
  ```

### Memory Checks

Use **Valgrind** to detect memory leaks and errors:
```bash
valgrind -v --leak-check=full ./channel <test_case_name> <iterations>
```

---

## Implementation Details

### Synchronization

The implementation ensures thread-safe operations using:
- **POSIX Mutexes**: For protecting shared resources.
- **Condition Variables**: For efficient waiting and signaling.
- Avoids busy-waiting, deadlocks, and race conditions.

### Buffer Management

The provided buffer utility functions include:
- `buffer_create`: Initializes a buffer.
- `buffer_add`: Adds data to the buffer.
- `buffer_remove`: Removes data from the buffer.
- `buffer_capacity`: Returns the buffer's total capacity.
- `buffer_current_size`: Returns the current number of elements in the buffer.

---

## Programming Guidelines

- Avoid spinning loops; use condition variables (e.g., `pthread_cond_wait`).
- Global variables are not allowed.
- Use only the following libraries:
  - POSIX Threads
  - POSIX Semaphores
  - Standard C Libraries (`malloc`, `free`, etc.)

---

## Debugging Tips

- Use **GDB** for analyzing crashes and deadlocks.
- Attach GDB to running processes to debug deadlocks (`gdb attach <PID>`).
- Use **Valgrind** for detecting memory leaks and invalid memory usage.

---

## Evaluation Criteria

Your implementation is evaluated based on:
1. **Correctness**: Ensuring thread-safe operations without race conditions.
2. **Memory Management**: Avoiding leaks and invalid accesses.
3. **Code Quality**: Clarity and modularity of the code.

---

## Resources

- [POSIX Threads Tutorial](https://hpc-tutorials.llnl.gov/posix/)
- [POSIX Semaphores Reference](https://pubs.opengroup.org/onlinepubs/7908799/xsh/semaphore.h.html)
- [Valgrind Documentation](https://valgrind.org/)

---

## Author
Implemented by **Vishvendra Reddy Bhoomidi** as part of the Concurrency Lab course by Timothy Zhu.
