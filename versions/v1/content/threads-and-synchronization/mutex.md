---
title: Mutual exclusion
assignment: mandatory
weight: 100
draft: false
---

<h2 class="subtitle">Mandatory assignment</h2>

![](/v1/images/threads-and-synchronization/mutex-inc-dec.png?width=555px)

In this assignment you will study different methods to enforce [mutual
exclusion](definitions#mutual-exclusion-mutex).


## Linux or macOS on Intel CPUs only

This assignment make use of [atomic
operations](definitions/#atomic-operations) available only on computers with
Intel CPUs running [Linux][wp-linux] or [macOS][wp-macos].

[wp-linux]: https://en.wikipedia.org/wiki/Linux

[wp-macos]: https://en.wikipedia.org/wiki/MacOS

## The department Linux system

If you are working on Windows or on Linux or macOS but with a non-Intel CPU, you
can use the department Linux system for this assignment. You may still use your
private computer to access the department Linux system using [SSH][ssh] but make sure
to log in to one of the [Linux hosts][linux-hosts].


[ssh]: http://www.it.uu.se/datordrift/faq/ssh?lang=enforce

[linux-hosts]: https://www.it.uu.se/datordrift/maskinpark/linux


## Overview

File to use
: `threads-synchronization-deadlock /mandatory/src/mutex.c`

Description
: In this program, a process creates a number of threads and waits for their
termination. The threads are divided into two sets **I** (increment) and **D**
(decrement). A shared variable which works as a `counter` is initialized to `0`
and then incremented by the threads in set **I** and decremented by the threads in
set **D**. The sum of all the increments is equal to the absolute value of the sum
of all the decrements.

Example
: There are five threads in set **I**, each incrementing the counter `20` times
by `2`. In total the counter will be incremented by `5*20*2 = 200`.
There are two threads in set **D**, each decrementing the counter `20` times by `5`. In
total the counter will be decremented by `2*20*5 = 200.`

Desired behavior
: When all threads are done executing the value of the shared counter should be
`0` (the same as value it was initialized with).

## A collection of tests

The provided program will execute 4 different tests. For each test, a different
approach  to synchronization will be used by the threads updating the shared
`counter` variable.  

## Compile and run

In the terminal, navigate to the `threads-synchronization-deadlock /mandatory` directory. Use
[make][wp-make] to compile the program.

[wp-make]: https://en.wikipedia.org/wiki/Make_(software)

``` text
make
```

The executable will be named `mutex` and placed in the `bin` sub directory. Run the program from the terminal.

``` text
./bin/mutex
```

## Summary of all the tests

At the end of the program output, a summary of the four tests will be printed. 

## Questions

Run the program several times and study the provided source code.

1. Does the program work according to the specification?
2. Can you predict the output before execution? Why?

## Identify the critical sections

Identify the [critical sections](definitions/#critical-section) in the
program. The critical sections should be as small as possible.


## Mutual exclusion

Your task is to make sure that at any time, at most one thread execute in a
critical section, i.e., access to the critical sections should be [mutual
exclusive][wp-mutex]. You will use three different methods to ensure that updates to the
shared counter variable appear to the system to occur instantaneously,
i.e., make sure updates are [atomic][wp-atomic].

[wp-mutex]: https://en.wikipedia.org/wiki/Mutual_exclusion
[wp-atomic]: https://en.wikipedia.org/wiki/Linearizability


## Test 0 - no synchronization

In test 0 the threads uses the functions `inc_no_syncx` and `dec_no_sync` to
update the shared variable `counter`. Updates are done with no
synchronization. 

You don't have to change or add any code to these two functions. This test is
used to demonsrate the effects of unsynchronized updates to shared variables. 

[wp-pthreads]: https://en.wikipedia.org/wiki/POSIX_Threads

## Test 1 - pthread mutex locks


The first synchronization option you will study is to use the
mutex lock provided by the [pthreads][wp-pthreads] library to enforce mutual exclusion
when accessing the shared counter variable.
Add the needed synchronization in the functions `inc_mutex` and `dec_mutex` to make
the program execute according to the desired behavior.

[wp-pthreads]: https://en.wikipedia.org/wiki/POSIX_Threads

### Reference

- [Pthreads Manual: Mutexes](https://man7.org/linux/man-pages/man3/pthread_mutex_lock.3p.html)


## Test 2 - Spin lock using test-and-set

An alternative to mutex locks is to use the
atomic [test-and-set][wp-tas] built-in provided by the [GCC][wp-gcc] compiler to implement
a spinlock.

{{% notice style="warning" title="Linux or macOS on Intel CPUs" %}}

This part of the assignment make use of [atomic
operations](definitions/#atomic-operations) available only on computers with
Intel CPUs running [Linux][wp-linux] or [macOS][wp-macos].

[wp-linux]: https://en.wikipedia.org/wiki/Linux

[wp-macos]: https://en.wikipedia.org/wiki/MacOS

If you are working on Windows or on Linux or macOS but with a non-Intel CPU, you
can use the department Linux system for this assignment. You may still use your
private computer to access the department Linux system using [SSH][ssh] but make sure
to log in to one of the [Linux hosts][linux-hosts].

[ssh]: http://www.it.uu.se/datordrift/faq/ssh?lang=enforce

[linux-hosts]: https://www.it.uu.se/datordrift/maskinpark/linux

{{% /notice %}}

You will use the following function to access the underlying test-and-set
operation.

int __sync_lock_test_and_set(int *lock, int value)
:  This **atomic** operation sets `*lock` to `value` and returns the previous
contents of `*lock`.

At the beginning of `mutex.c` you find the following declaration.

``` C
/* Shared variable used to implement a spinlock */
volatile int lock = false;

```

Todo:

- Use the shared `lock` variable to implement the `spin_lock()` and `spin_unlock()`
operations.
- Change the functions `inc_tas_spinlock` and `dec_tas_spinlock` to use use the
test-and-set spinlock to enforce mutual exclusion.

[wp-tas]: https://en.wikipedia.org/wiki/Test-and-set
[wp-cas]: http://en.wikipedia.org/wiki/Compare-and-swap

[wp-gcc]: https://en.wikipedia.org/wiki/GNU_Compiler_Collection

### Reference

- [GCC Manual (version 5.4.0): Atomic built-ins](https://gcc.gnu.org/onlinedocs/gcc-5.4.0/gcc/_005f_005fsync-Builtins.html#g_t_005f_005fsync-Builtins)


## Test 3 - atomic addition and subtraction

A third option is to use the atomic addition and subtraction GCC built-ins.

{{% notice style="warning" title="Linux or macOS on Intel CPUs" %}}

This part of the assignment make use of [atomic
operations](definitions/#atomic-operations) available only on computers with
Intel CPUs running [Linux][wp-linux] or [macOS][wp-macos].

[wp-linux]: https://en.wikipedia.org/wiki/Linux

[wp-macos]: https://en.wikipedia.org/wiki/MacOS

If you are working on Windows or on Linux or macOS but with a non-Intel CPU, you
can use the department Linux system for this assignment. You may still use your
private computer to access the department Linux system using [SSH][ssh] but make sure
to log in to one of the [Linux hosts][linux-hosts].

[ssh]: http://www.it.uu.se/datordrift/faq/ssh?lang=enforce

[linux-hosts]: https://www.it.uu.se/datordrift/maskinpark/linux

{{% /notice %}}

You will use the following functions to access the underlying atomic increment
and decrement operations.

int __sync_fetch_and_add(int *counter, int n)
:  This **atomic** operation increments `*counter` by `n` and returns the
previous value of `*counter`.

int __sync_fetch_and_sub(int *counter, int n)
:  This **atomic** operation decrements `*counter` by `n` and returns the
previous value of `*counter`.


Change the functions `inc_atomic` and `dec_atomic` to use atomic addition and
subtraction to make the program execute according to the desired behavior.

### Reference

- [GCC Manual (version 5.4.0): Atomic built-ins](https://gcc.gnu.org/onlinedocs/gcc-5.4.0/gcc/_005f_005fsync-Builtins.html#g_t_005f_005fsync-Builtins)

## Evaluation

Study the test summary table printed after all tests have completed.

- Make sure all test but test 0 (no synchronization) are successful. 
- Compare the execution times for the 4 tests. 
    - Which method of synchronization is the fasted?
    - Which method of synchronization is the slowest?
    - Which method of synchronization is neither the fastest, nor the slowest?
    - Why do you think the speed of the  three methods of synchronization are
      ordered like this?


## Code grading questions

Here are a few examples of questions that you should be able to answer, discuss
and relate to the source code of you solution during the code grading.

Explain the following concepts and relate them to the source code and the behavior of the program.

- Critical section. 
- Mutual exclusion (mutex).
- Race condition.
- Data race.

Locks and semaphores:

- What is the purpose of mutex locks?
- If yoy had to make a choice between using a semaphore or a mutex to enforce
  mutex, what would you recommend and why? 
- How do you construct a spinlock using the atomic  `test-and-set` instruction?

Performance analysis: 

- Discuss and analyze the results in test summary table. 
