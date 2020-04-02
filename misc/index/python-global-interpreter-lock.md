---
description: 'https://realpython.com/python-gil/'
---

# Python Global Interpreter Lock

Disclaimer: This note is entirely based on the blog post above.

### What is GIL?

The Python Global Interpreter Lock or [GIL](https://wiki.python.org/moin/GlobalInterpreterLock), in simple words, is a mutex \(or a lock\) that allows only one thread to hold the control of the Python interpreter.

### Why do we need it in Python?

Python uses reference counting for memory management. It means that objects created in Python have a reference count variable that keeps track of the number of references that point to the object. When this count reaches zero, the memory occupied by the object is released.

```text
>>> import sys
>>> a = []
>>> b = a
>>> sys.getrefcount(a)
3
```

The problem is that this reference count variable needed protection from race conditions where two threads increase or decrease its value simultaneously. If this happens, it can cause either leaked memory that is never released or, even worse, incorrectly release the memory while a reference to that object still exists. This can can cause crashes or other “weird” bugs in your Python programs.

This reference count variable can be kept safe by adding _locks_ to all data structures that are shared across threads so that they are not modified inconsistently.

### The impact on multi-threaded Python programs <a id="the-impact-on-multi-threaded-python-programs"></a>

```text
# single_threaded.py
import time
from threading import Thread

COUNT = 100000000

def countdown(n):
    while n>0:
        n -= 1

start = time.time()
countdown(COUNT)
end = time.time()

print('Time taken in seconds -', end - start) # Took 6.608175992965698 on my Mac
```

Now if we modified the code a bit to do to the same countdown using two threads in parallel: 

```text
# multi_threaded.py
import time
from threading import Thread

COUNT = 100000000

def countdown(n):
    while n>0:
        n -= 1

t1 = Thread(target=countdown, args=(COUNT//2,))
t2 = Thread(target=countdown, args=(COUNT//2,))

start = time.time()
t1.start()
t2.start()
t1.join()
t2.join()
end = time.time()

print('Time taken in seconds -', end - start) # Took 6.042619943618774 on my Mac
```

As you can see, both versions take almost same amount of time to finish. In the multi-threaded version the GIL prevented the CPU-bound threads from executing in parallel.

### How to deal with Python’s GIL <a id="how-to-deal-with-pythons-gil"></a>

The most popular way is to use a multi-processing approach where you use multiple processes instead of threads. Each Python process gets its own Python interpreter and memory space so the GIL won’t be a problem.

```text
from multiprocessing import Pool
import time

COUNT = 50000000
def countdown(n):
    while n>0:
        n -= 1

if __name__ == '__main__':
    pool = Pool(processes=2)
    start = time.time()
    r1 = pool.apply_async(countdown, [COUNT//2])
    r2 = pool.apply_async(countdown, [COUNT//2])
    pool.close()
    pool.join()
    end = time.time()
    print('Time taken in seconds -', end - start) # Took 4.294095039367676
```

We can observe a decent performance increase compared to the multi-threaded version.

