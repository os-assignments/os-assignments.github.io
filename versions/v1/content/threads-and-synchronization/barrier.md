---
title: Barrier synchronization
assignment: mandatory
weight: 200
draft: false
---

<h2 class="subtitle">Mandatory assignment</h2>

![](/v1/images/module-4/barrier.png?width=433px)

A mutex lock is meant to be taken and released, always in that order, by each
task that uses the shared resource it protects.
In general, semaphores should not be used to enforce mutual exclusion.
The correct use of a semaphore is to signaling from one task to another, i.e., a
task  either signal or wait, not both.

In this assignment you will solve the rendezvous problem using semaphores. This
problem clearly demonstrates the type of synchronization that can be provided by
semaphores.

## Supported platforms

The provided code has been developed and tested on the department Linux system
and macOS. Most likely you will be able to use any reasonable modern version of
Linux. 

<!--

If you use Windows you must use the department Linux system for this assignment.
You may still use your private computer to access the department Linux system
using [SSH][ssh] but make sure to log in to one of
the [Linux hosts][linux-hosts].

[ssh]: http://www.it.uu.se/datordrift/faq/ssh?lang=enforce

[linux-hosts]: https://www.it.uu.se/datordrift/maskinpark/linux

[piazza]: https://piazza.com/class/jqzauvl4hqm6v9?cid=80#

-->

## Overview

File to use
: `module-4/mandatory/src/rendezvous.c`

Description
: In this program, a process creates two threads, and waits for their termination.
Each thread performs five iterations. In each iteration the threads  print their
iteration number and sleeps for some random amount of time.

## Compile and run

In the terminal, navigate to the `module-4/mandatory` directory. Use [make][wp-make] to compile the program.

[wp-make]: https://en.wikipedia.org/wiki/Make_(software)

``` text
$ make
```

The executable will be named `rendezvous` and placed in the `bin` sub directory. Run the program from the terminal.

``` text
$ ./bin/rendezvous
```

## Questions

Run the programs several times and study the source code. Think about the
following questions.

- Could you predict the output before execution? Why?
- Adjust the sleeping duration of one thread (or both threads) to have a
different output (ie. another interleaving of the tasks traces). Could you
predict this output? Why?

## Rendezvous

Rendezvous or rendez-vous (French pronunciation: [ʁɑ̃devu]) refers to a planned
meeting between two or more parties at a specific time and place.
[^wp-rendezvous]

[^wp-rendezvous]: [Wikipedia - Rendezvous](https://en.wikipedia.org/wiki/Rendezvous)

## Desired behavior

We now want to make the two threads have a rendezvous after
each iteration, i.e., the two threads **A** and **B** should perform their iterations in
lockstep. Lockstep means that they both
first perform iteration **0**, then iteration **1**, then iteration **2**, etc. 
For each iteration the order between **A** and **B** should not be restricted. 

- Either **A** completes iteration **i** before **B** or **B** completes
  iteration **i** before **A**. 
- A thread is not allowed to start itertion **i+1** until the other thread also
  completed iteration **i**.

An example for iteration 0 and 1 is shown below. 

![](/v1/images/module-4/rendezvous-a-b-two-iterations.png?width=666px)

In the above example, the two threads A and B compete (execute concurrently) and
one will be first to reach the rendezvous point. For iteration 0, thread B is
first to reach the rendezvous point and must wait for thread B to also reach the
rendezvous point. Once the two threads have been synchronized at the rendezvous
point, they once again compete during iteration 1 to be the first to reach the
next rendezvous point. In the above example, for iteration 1, thread A is first to reach the
rendezvous point and must wait for thread B.



## Portable semaphores

Use the [psem](psem) semaphores to enforce rendezvous between the two threads. .

## Number of semaphores

How many semaphores are needed to make the two threads have a rendezvous after
each iteration?

- Can rendezvous be achieved using a single semaphore?
- Do you need to use two semaphores?

## Declare global semaphore variables

For simplicity, declare all `psem_t` semaphore variables globally. 

## Initial semaphore values

Initialize your semaphore(s) in the beginning of `main()`. 

- Should the semaphore(s) be initialized with counter value 0, 1 or some other value?

## Destroy semaphores after use

At the end of `main()`, don't forget to destroy any semaphores you have initialized. 

## Compile and run

Compile and run the program.

``` text
$ make
$ ./bin/rendezvous
```

## Example of invalid output

This is an example of an invalid execution trace.

``` c
A0
B0
B1
A1
B2
B3   // ERROR: should be A2
A2
```

## Example of valid output


This is an example of a valid  execution trace.

``` c
A0
B0
B1
A1
B2
A2
A3
B3
```

## Change the relative speeds of the threads

Change the threads sleeping durations.

- Does the output change?
- What are the possible outputs?

## Code grading questions

Here are a few examples of questions that you should be able to answer, discuss
and relate to the source code of you solution during the code grading.

- Explain the concept of rendezvous.
- What will happen when you wait on a semaphore?
- What will happen when you signal on a semaphore?
- How can semaphores be used to enforce rendezvous between two threads?
- How are mutex locks different compared to semaphores?
- Why can't mutex locks be used to solve the rendezvous problem?
