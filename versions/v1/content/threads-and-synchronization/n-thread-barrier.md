---
title: N thread barrier
assignment: higher-grade
weight: 580
draft: false
---

<h2 class="subtitle">Optional assignment for higher grade</h2>

![](/v1/images/threads-and-synchronization/n-thread-barrier.png?width=633px)

In the [two-thread-barrier](two-thread-barrier) assignment you solved the barrier synchronization problem for two threads. 
In this higher grade assignment you will solve the barrier synchronization for an arbitrary number of threads. 

## Supported platforms

The provided code has been developed and tested on the department Linux system
and macOS. Most likely you will be able to use any reasonable modern version of
Linux. 


## Overview

In the `threads-and-synchronization/higher-grade/src` directory you find the following files. 

n_thread_barrier.h
: Header file with the API to the n thread barrier. Here you must 

n_thread_barrier.c
: The implementation of the n thread barrier. 

n_thread_barrier_test.c
: A program testing the implemented n thread barrier. 


## The barrier API

The n thread barrier API is defined in `n_thread_barrier.h`.

``` C 
/**
 * barrier_init()
 *   Creates a new barrier. 
 * 
 * barrier 
 *   Pointer to a barrier. 
 * 
 * count
 *   Number of threads participating in the barrier. 
*/
void barrier_init(barrier_t *barrier, int count);

/**
 * barrier_destroy()
 *   Destroys a barrier, freeing the resources it might hold.     
 * 
 * barrier: 
 *   Pointer to a barrier. 
 * 
 * */
void barrier_destroy(barrier_t *barrier);

/**
 *  barrier_wait()
 *    Will block the calling thread until all other threads also 
 *    executed barrier_wait().
 * 
 *  barrier: 
 *   Pointer to a barrier. 
 *  
*/
void barrier_wait(barrier_t *barrier);
```

## The barrier structure

In `n_thread_barrier.h` you find the declaration of the `barrier_t` structure. 

``` C
typedef struct {
    // Number of threads participating in the barrier. 
    unsigned int count;  
} barrier_t;
```

What more is needed to be added to the structure to implement the barrier? For example, do you need to add:

- one ore more `psem` semaphores?
- one ore more pthread mutex locks?
- anything else?

## Pthread mutex locks

This is how you can declare and initialize a [Pthread mutex lock][pthread-mutex]. 


``` C
pthread_mutex_t mutex;

if (pthread_mutex_init(&mutex, NULL) < 0) {
  perror("Init mutex lock");
  exit(EXIT_FAILURE);
}
```

[pthread-mutex]: https://man7.org/linux/man-pages/man3/pthread_mutex_lock.3p.html

## Portable semaphores

This is how you declare and initialize a portable [psem](psem) semaphore. 

``` C

psem_t *semaphore;

semaphore = psem_init(0);
```


## Implementing the barrier API

Your task is to implement the barrier API in the `n_thread_barrier.c` file. 

## Testing the barrier API implementation 

In `n_thread_barrier_test.c` you find a working program testing your implementation. 

- The threads are created in the `main` function.

- Each thread executes the `thread` function. 

- In the `thread` function, instead of printing their 
identity (0, 1, 2, ..., N-1) directly, the threads uses the provided `trace` function. 

- The `trace` functions prints the thread identity and keeps track of the next valid
thread identity in the traced sequence. If a thread jumps over the barrier to early,
an error is reported and the process is terminated. 


## Compile and run the test

Compile:

``` text
make
```

, and run the test program: 

``` text
./bin/n_thread_test
```

## Example of incorrect barrier synchronization

This is an example of incorrect barrier synchronization.

``` text
5 threads T0, ..., T4 doing 3 iterations each in lockstep.

Iteration 0

  T0
  T1
  T2
  T3
  T4

Iteration 1

  T1
  T2
  T3
  T3 <=== ERROR: Jumped over the barrier to early!
```

## Example of correct barrier synchronization

This is an example of correct barrier synchronization.

``` text
5 threads T0, ..., T4 doing 3 iterations each in lockstep.

Iteration 0

  T2
  T4
  T0
  T1
  T3

Iteration 1

  T3
  T0
  T4
  T1
  T2

Iteration 2

  T2
  T0
  T1
  T4
  T3

SUCCESS: All iterations done!
```

## Code grading questions

TODO: Add code grading questions here. 