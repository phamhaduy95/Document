when dealing with concatenating strings, there are four common ways in python:
1.  ==+== operator: slowest
2. string format syntax: great for its flexibility and readability but still slow
3. join the fastest. Good for medium-size list of strings
4. `io.stringIO()`  support buffer to optimize space efficiency, best for large dataset.

call flush will make python release data immediately instead of waiting till buffer is full or program is stopped.


why we need buffer when we try to read or write a file
 reduce system call to write or read data from disk through batching.
most OS h
you want to immediate output such as logging


| Benefit                          | Explanation                                                                             |
| -------------------------------- | --------------------------------------------------------------------------------------- |
| 🚀 **Performance**               | Reduces the number of system calls (e.g., to disk or network) by batching reads/writes  |
| 🧠 **Efficiency**                | Writing/reading 4 KB at once is far more efficient than 4000 separate 1-byte operations |
| ⏱ **Lower latency (in total)**   | Even though data may wait briefly in memory, the **overall speed is faster**            |
| 🔁 **Enables seeking & peeking** | Buffered objects can read ahead and rewind internally (important for file parsers)      |
| 💻 **OS-friendly**               | Most operating systems optimize for buffered I/O; unbuffered can hurt performance       |
