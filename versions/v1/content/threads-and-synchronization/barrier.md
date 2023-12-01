---
title: Barrier synchronization
assignment: mandatory
weight: 200
draft: false
---

<h2 class="subtitle">Mandatory assignment</h2>

![](/v1/images/threads-and-synchronization/barrier.png?width=433px)

A mutex lock is meant to first be taken and then released, always in that order, by each
task that uses the shared resource it protects.
In general, semaphores should not be used to enforce mutual exclusion.
The correct use of a semaphore is to signaling from one task to another, i.e., a
task either signal or wait on a semaphore, not both.

In this assignment you will solve the barrier problem using semaphores. This
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
: `threads-and-synchronization/mandatory/src/barrier.c`

Description
: In this program, a process creates two threads A and B, and waits for their termination.
Each thread performs five iterations. In each iteration the threads print their name (A or B)
and sleeps for some random amount of time. For each iteration the order between the threads should not be restricted. 

![](/v1/images/threads-and-synchronization/no-barrier.png?width=433px)


## Compile and run

In the terminal, navigate to the `threads-and-synchronization/mandatory` directory. 
Use [make][wp-make] to compile the program.

[wp-make]: https://en.wikipedia.org/wiki/Make_(software)

``` text
make
```

The executable will be named `barrier` and placed in the `bin` sub directory. 
Run the program from the terminal.

``` text
./bin/barrier
```

## Questions

Run the programs several times and study the source code. Think about the
following questions.

- Could you predict the output before execution? Why?
- Adjust the sleeping duration of one thread (or both threads) to have a
different output (ie. another interleaving of the tasks traces). Could you
predict this output? Why?

## Rendezvous

Rendezvous or rendezvous (French pronunciation: [ʁɑ̃devu]) refers to a planned
meeting between two or more parties at a specific time and place.
[^wp-rendezvous]

[^wp-rendezvous]: [Wikipedia - Rendezvous](https://en.wikipedia.org/wiki/Rendezvous)

## Barrier synchronization (desired behavior)

We now want to put in a barrier in each thread that enforces the two threads to have rendezvous 
at the barrier in each iteration. 

![](/v1/images/threads-and-synchronization/barrier.png?width=433px)

The first thread to reach the barrier must wait for the other thread to also reach the barrier. 
Once both threads have reached the barrier the threads are allowed to continue with the next iteration.
Examples of execution traces:
- ABBABAABBA (valid)
- ABB**B**ABBAAB (invalid)

{{% notice style="info" title="Lock-step and rendezvous" %}}

Other names for barrier synchronization are lock-step and rendezvous. 

{{% /notice %}}

## Barrier implementation

Your task is to use semaphores to enforce barrier synchronization between the two thread A and B. 

![](/v1/images/threads-and-synchronization/barrier-template.png?width=433px)

Before you continue think about the following questions. 

- What needs to be initialized? 
- What needs to be done by each thread inside the barrier?

## Portable semaphores

Use the [psem](psem) semaphores to enforce rendezvous between the two threads. .

## Number of semaphores

How many semaphores are needed to enforce barrier synchronization between two threads?

- Can this be achieved using a single semaphore?
- Do you need to use two semaphores?
- Do you need more than two semaphores?

## Declare global semaphore variables

For simplicity, declare all `psem_t` semaphore variables globally. 

## Initial semaphore values

Initialize your semaphore(s) in the beginning of `main()`. 

- Should the semaphore(s) be initialized with counter value 0, 1 or some other value?

## Destroy semaphores after use

At the end of `main()`, don't forget to destroy any semaphores you have initialized. 

## Compile and run

Compile:

``` text
make
```

, and run the program: 

``` text
./bin/rendezvous
```

## Example of invalid output

This is an example of an invalid execution trace.

``` c
A
B
B
A
B
B   // ERROR: should be A
A
```

## Example of valid output


This is an example of a valid  execution trace.

``` c
A
B
B
A
B
A
A
B
```

## Change the relative speeds of the threads

Change the threads sleeping durations.

- Does the output change?
- What are the possible outputs?

## Code grading questions

Here are a few examples of questions that you should be able to answer, discuss
and relate to the source code of you solution during the code grading.

- How are mutex locks different compared to semaphores?
- In general, what will happen when you wait on a semaphore?
- In general, What will happen when you signal on a semaphore?
- Explain the barrier synchronization concept.
- How can semaphores be used to enforce barrier synchronization between two threads?
- Why can't mutex locks be used as drop-in replacements for the semaphores in your solution 
with lock instead of wait and unlock instead of signal?
