---
tags:
  - concurrency
  - multithreading
  - python
---

# Concurrency
> [!INFO]- References
> [LeetCode Post](https://leetcode.com/problems/print-in-order/discuss/335939/5-Python-threading-solutions-(Barrier-Lock-Event-Semaphore-Condition)-with-explanation)

## Example Problem
You are given a class, `Foo`, that has 3 functions:

 * `first(printFirst: Callable)`
 * `second(printSecond: Callable)`
 * `third(printThird: Callable)`

Design a mechanism and modify the program to ensure that `printSecond()` executes after `printFirst()` and `printThird()` executes after `printSecond()`.

## [Barrier](https://docs.python.org/3/library/threading.html#barrier-objects)
A #barrier forces a thread to wait for all parties to call the Barrier object. Once all parties call the Barrier to wait, they are all released simultaneously.

### Solution
```python
from threading import Barrier

class Foo:
    def __init__(self):
        self.first_barrier = Barrier(2)
        self.second_barrier = Barrier(2)
            
    def first(self, printFirst):
        printFirst()
        self.first_barrier.wait()
        
    def second(self, printSecond):
        self.first_barrier.wait()
        printSecond()
        self.second_barrier.wait()
            
    def third(self, printThird):
        self.second_barrier.wait()
        printThird()
```

## [ Lock](https://docs.python.org/3/library/threading.html#lock-objects)
A primitive #lock is a synchronization primitive that is not owned by a particular thread when locked. In Python, it is currently the lowest level synchronization primitive available, implemented directly by the [`_thread`](https://docs.python.org/3/library/_thread.html#module-_thread "_thread: Low-level threading API.") extension module.

A primitive lock is in one of two states, “locked” or “unlocked”. It is created in the unlocked state. When the state is unlocked, acquiring the lock sets it to the locked state. When the state is locked, acquiring the lock blocks until it is released.

A lock be considered equivalent to a ***mutex***.

### Solution
```python
from threading import Lock

class Foo:
    def __init__(self):
        self.locks = (Lock(),Lock())
        self.locks[0].acquire()
        self.locks[1].acquire()
        
    def first(self, printFirst):
        printFirst()
        self.locks[0].release()
        
    def second(self, printSecond):
        with self.locks[0]:
            printSecond()
            self.locks[1].release()
            
            
    def third(self, printThird):
        with self.locks[1]:
            printThird()
```

## [Event](https://docs.python.org/3/library/threading.html#event-objects)
An #event allows two threads to communicate by having one set the event and having the other wait for the thread.

### Solution
```python
from threading import Event

class Foo:
    def __init__(self):
        self.done = (Event(),Event())
        
    def first(self, printFirst):
        printFirst()
        self.done[0].set()
        
    def second(self, printSecond):
        self.done[0].wait()
        printSecond()
        self.done[1].set()
            
    def third(self, printThird):
        self.done[1].wait()
        printThird()
```

## [Semaphore](https://docs.python.org/3/library/threading.html#semaphore-objects)
A #semaphore has an internal counter that is decremented every time it is acquired and incremented every time it is released.

A semaphore with a count of 1 is consider a #mutex.

### Solution
```python
from threading import Semaphore

class Foo:
    def __init__(self):
        self.gates = (Semaphore(0),Semaphore(0))
        
    def first(self, printFirst):
        printFirst()
        self.gates[0].release()
        
    def second(self, printSecond):
        with self.gates[0]:
            printSecond()
            self.gates[1].release()
            
    def third(self, printThird):
        with self.gates[1]:
            printThird()
```

## [Condition](https://docs.python.org/3/library/threading.html#condition-objects)
A #condition variables are **synchronization primitives that enable threads to wait until a particular condition occurs**.

### Solution
```python
from threading import Condition

class Foo:
    def __init__(self):
        self.exec_condition = Condition()
        self.order = 0
        self.first_finish = lambda: self.order == 1
        self.second_finish = lambda: self.order == 2

    def first(self, printFirst):
        with self.exec_condition:
            printFirst()
            self.order = 1
            self.exec_condition.notify(2)

    def second(self, printSecond):
        with self.exec_condition:
            self.exec_condition.wait_for(self.first_finish)
            printSecond()
            self.order = 2
            self.exec_condition.notify()

    def third(self, printThird):
        with self.exec_condition:
            self.exec_condition.wait_for(self.second_finish)
            printThird()
```
