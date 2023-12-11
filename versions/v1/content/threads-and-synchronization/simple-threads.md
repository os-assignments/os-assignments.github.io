---
title: "Simple Threads"
assignment: higher-grade
weight: 600
draft: false
---

<h2 class="subtitle">Optional assignment for higher grade</h2>

![](/v1/images/threads-and-synchronization/higher-grade-overview.png?width=666px)

In this assignment you will implement a simplified version of many-to-one user level
threads in form of a library that we give the name Simple Threads.

## Preparations 

Before you continue make sure you've read the [implementing threads](implementing-threads) page.
Among the slides in Studium you also find self study material about implementing threads. 

## Git 

You should already have [cloned](clone-repository) the [threads-and-synchronization][repo] repository.


[repo]: https://github.com/os-assignments/threads-synchronization-deadlock.git


## Manage execution contexts 

The `ucontext.h` header file defines the `ucontext_t` type as a structure that
can be used to store the execution context of a user-level thread in user space.
The same header file also defines a small set of functions used to manipulate
execution contexts. Read the following manual pages.

- [ucontext.h][man-ucontext]
- [getcontext()][man-getcontext]
- [setcontext()][man-setcontext]
- [makecontext()][man-makecontext]
- [swapcontext()][man-swapcontext] 

[man-ucontext]: http://pubs.opengroup.org/onlinepubs/7908799/xsh/getcontext.html
[man-getcontext]: http://pubs.opengroup.org/onlinepubs/7908799/xsh/getcontext.html
[man-setcontext]: http://pubs.opengroup.org/onlinepubs/7908799/xsh/setcontext.html
[man-makecontext]: http://pubs.opengroup.org/onlinepubs/7908799/xsh/makecontext.html
[man-swapcontext]: http://pubs.opengroup.org/onlinepubs/7908799/xsh/swapcontext.html 

### Example

In `examples/src/contexts.c` you find an example program demonstrating
how to create and manipulate  execution contexts. 
Study the source. To compile, navigate to the `examples` directory in the terminal, type `make`
and press enter.

``` bash session
make
```

To run: 

``` bash session
./bin/contexts
```

## Get started 

In `higher-grade/src` you find the following files. 

sthreads.h
: Header file specifying the Simple Threads [API](https://en.wikipedia.org/wiki/Application_programming_interface).

sthreads.c
: Implementation of the Simple Threads API.

sthreads_test.c
: A program used to test the Simple Threads API.

Study the source and pay attention to all comments. 

## First compile and test run

In the terminal, navigate to the `higher-grade` directory. Use make to
compile. 

``` bash session
make
```

Run the test program. 

``` bash session
./bin/sthreads_test
```

The program prints the following to terminal and terminates. 

``` bash session
==== Test program for the Simple Threads API ====
```
## Cooperative scheduling (grade 4)

For grade 4 you must implement **all** the **functions** in the Simple Threads API
**except** `done()` and `join`. This includes **cooperative scheduling** where
threads `yield()` control back to the thread manager. 

The test program's `main()` thread and all threads
created by `main()` share a single
kernel-level thread. The threads must call `yield()` to pass control to the thread
manager. When
calling `yield()` one of the ready threads is selected to execute and changes
state from ready to running. The thread calling `yield()` changes state from
running to ready. 

![](/v1/images/threads-and-synchronization/cooperative.png?height=333px)

{{% notice style="info" title="Non-terminating threads" class="tip" %}}
For grade 4 you may assume the main thread and all other threads are
non-terminating loops. 
{{% /notice %}}

## Preemptive scheduling (grade 5)

For grade 5 you must also implement the `done()` and `join()` functions in the
Simple Threads API. In addition to cooperative scheduling with `yield()` you must also implement
**preemptive scheduling**. 

![](/v1/images/threads-and-synchronization/cooperative-and-preemptive.png?height=333px)

If a thread doesn't call `yield()` within its time slice it will be preempted
and one of the threads in the ready queue will be resumed. The preempted thread
changes state from running to ready. The resumed thread changes state from ready
to running. 

### Timer

To implement preemptive scheduling you need to set a timer. When the timer
expires the kernel will send a signal to the process. You must register a signal
handler for the timer signal. In the signal handler you must figure out a way to
suspend the running thread and resume one of the threads in the ready queue. 

### Timer example

In `examples/src/timer.c` you find an example of how to set a timer.
When the timer expires the kernel sends a signal to the process. A signal
handler is used to catch the timer signal. 

