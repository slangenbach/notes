# Async techniques in Python

Notes on the [course][1] from TalkPython Training.

## General

- A process is an independent program execution instance with its own memory and resources. Processes have at least one thread and are isolated from each other. Creating a process is heavy weight 
- A thread is light weight execution unit within a process. Threads share the process memory and resources
- Use `ProcessPoolExecutor` for CPU-intensive work (do things faster) and `ThreadPoolExecutor` or [asyncio][2] for IO-intensive work (do more at once)
- Use asyncio as a lightweight approach to async development when scalability is important
- From Python 3.14 onwards, the GIL can be disabled to enable [free treading][3], so threads can be used to do things faster **and** more at once
- If optimizing for performance, always think about the upper bound of improvement, _before_ implementing logic asynchronously

## AsyncIO

- The event loop orchestrates tasks
- Prefixing a function with `async` makes it an async or coroutine function
- Calling an async function returns a coroutine object (~ coroutine)
- Coroutines are build on generators, which can regarded as restartable functions
- Tasks are coroutines tied to an event loop


[1]: https://training.talkpython.fm/courses/details/python-concurrency-deep-dive
[2]: https://docs.python.org/3/howto/a-conceptual-overview-of-asyncio.html#a-conceptual-overview-of-asyncio
[3]: https://docs.python.org/3/howto/free-threading-python.html
