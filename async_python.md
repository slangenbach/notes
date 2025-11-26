# Async techniques in Python

## General

- A process is an independent program execution instance with its own memory and resources. Processes have at least one thread and are isolated from each other. Creating a process is heavy weight 
- A thread is light weight execution unit within a process. Threads share the process memory and resources
- Use `ProcessPoolExecutor` for CPU-intensive work, `ThreadPoolExecutor` or async/await for IO-intensive work
- Use async/await as lightweight approach to async development when scalability is important
