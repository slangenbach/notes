# Async techniques in Python

Notes on the [course][1] from TalkPython Training.

## General

- A process is an independent program execution instance with its own memory and resources. Processes have at least one thread and are isolated from each other. Creating a process is heavy weight 
- A thread is light weight execution unit within a process. Threads share the process memory and resources
- Use **processes** for CPU-intensive work (do things faster) and **threads** or [asyncio][2] for IO-intensive work (do more at once)
- Use asyncio as a lightweight approach to async development when you work with libraries supporting it -> _Use async when you can, threads when you must!_
- From Python 3.14 onwards, the GIL can be disabled to enable [free treading][3], so threads can be used to do things faster **and** more at once
- If optimizing for performance, always think about the upper bound of improvement, _before_ implementing logic asynchronously
- Consider [alternative event loop implementations][4], if dealing with lots of events

## AsyncIO

- The event loop orchestrates tasks
- Prefixing a function with `async` makes it an async or coroutine function
- Calling an async function returns a coroutine object (~ coroutine)
- Coroutines are build on generators, which can regarded as restartable functions
- Tasks are coroutines tied to an event loop

### Web Requests

- Use async http clients like [aiohttp][5] or [httpx][6] to make requests
- Consider using [aiofiles][7] to handle local files in async applications

## Threads

- [Threads][8] work with the fork/join pattern
- We can create threads via `t = threading.Thread(target=<NAME_OF_FUNCTION>, args=(), kwargs={})` and start them via `t.start()`
- If `daemon=True` the thread runs in the background and is immediately shut down if the main process shuts down (without any clean up)
- If we want to wait for all threads to finish, use need to join them via `t.join()`
- We may use a dedicated cancellation thread to check for signals to cancel all active threads, by first checking if any thread is alive, joining on them with a small wait time, and then checking if the cancellation thread is alive
- An alternative to using the threading module directly is to use [ThreadPoolExecutor][9] instead

### Thread Safety

- Thread Safety is about avoiding temporary, invalid states in programs using threading
- Use Lock[10] per default
- Consider using reentrant lock ([RLock][11]) only if the same threads needs to re-enter the same lock before releasing it
- Using fine-grained locks often adds complexity and only yields minimal performance improvements

## Multiprocessing

- In order to use multiprocessing, first create a pool via `multiprocessing.Pool()`
- Then create tasks via `task = pool.apply_async(func=<FUNC_NAME>, args=(<ARGS>))`
- Finally close the pool `pool.close()` and join `pool.join()`
- If we need results from tasks, retrieve them via `task.get()`
- An alternative to using the multiprocessing module directly is to use [ProcessPoolExecutor][12] instead


[1]: https://training.talkpython.fm/courses/details/python-concurrency-deep-dive
[2]: https://docs.python.org/3/howto/a-conceptual-overview-of-asyncio.html#a-conceptual-overview-of-asyncio
[3]: https://docs.python.org/3/howto/free-threading-python.html
[4]: https://github.com/MagicStack/uvloop
[5]: https://docs.aiohttp.org/en/stable/index.html
[6]: https://github.com/encode/httpx/
[7]: https://github.com/Tinche/aiofiles
[8]: https://docs.python.org/3/library/threading.html
[9]: https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor
[10]: https://docs.python.org/3/library/threading.html#lock-objects
[11]: https://docs.python.org/3/library/threading.html#rlock-objects
[12]: https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor
