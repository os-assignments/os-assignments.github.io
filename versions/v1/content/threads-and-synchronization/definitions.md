---
title: Definitions
weight: 10
draft: false
---

## Concurrency 

Concurrency refers to the ability of different parts or units of a program,
algorithm, or problem to be executed out-of-order or in partial order, without
affecting the final outcome. This allows for parallel execution of the
concurrent units, which can significantly improve overall speed of the execution
in multi-processor and multi-core systems. [^wp-concurrency]

[^wp-concurrency]: [Wikipedia - Concurrency][wp-href-concurrency]

[wp-href-concurrency]: <https://en.wikipedia.org/wiki/Concurrency_(computer_science)> 

- Two tasks are concurrent if the order in which the two tasks are executed in
time is not predetermined. 
- Tasks are actually split up into chunks that share the processor with chunks
from other tasks by interleaving the execution in a time-slicing way. Tasks can
start, run, and complete in overlapping time periods.

## Process

To run a program the operating system must allocate memory and creates a
process. A processes have stack, heap, data segment and text segment in user
space. The CPU context and file descriptor table are kept in kernel space. When
executing the program, the program counter (PC) jumps around in the text
segment.  


![](/v1/images/threads-and-synchronization/process.png)


## Treads

A process can have one ore more threads of execution. Threads share heap, data
segment, text segment and file descriptor table but have separate stacks and
CPU contexts. Each thread have a private program counter and threads executes
concurrently. Depending on how threads are implemented, the CPU contexts can be
stored in user space or kernel space. In the below figure a process with three
threads is shown.

![](/v1/images/threads-and-synchronization/process-with-three-threads.png)

## Concurrent tasks

If not explicitly stated otherwise, the term task will be used to refer to a
concurrent unit of execution such as a thread or a process. 

## Atomic operations

In concurrent programming, an operation (or set of operations) is **atomic** if
it appears to the rest of the system to occur at once without being interrupted.
Other words used synonymously with atomic
are: **linearizable**, **indivisible** or **uninterruptible**.
[^wp-linearizability]

[^wp-linearizability]: [Wikipedia - Linearizability](https://en.wikipedia.org/wiki/Linearizability)

## Non-atomic operations

Incrementing and decrementing a variable are examples of non-atomic operations.
The following C expression, 

``` C
X++;
```
, is translated by the compiler to three instructions: 

1. **load** `X` from memory into a CPU register.
2. **increment** `X` and save result in a CPU register.
3. **store** result back to memory.

Similarly, the following C expression,

``` C
X--;
```
, is translated by the compiler to three instructions: 

1. **load** `X` from memory into a CPU register.
2. **decrement** `X` and save result in a CPU register.
3. **store** the new value of `X` back to memory.


## Race condition

A race condition or race hazard is the behaviour of an electronic, software or
other system where the output is dependent on the sequence or timing of other
uncontrollable events. It becomes a bug when events do not happen in the
intended order. The term originates with the idea of two signals racing each
other to influence the output first. [^wp-race-condition]
 
[^wp-race-condition]: [Wikipedia - Race condition](https://en.wikipedia.org/wiki/Race_condition)

## Data race

A data race occurs when two instructions from different threads access the same
memory location and:

- at least one of these accesses is a write
- and there is no synchronization that is mandating any particular order among
these accesses.


## Critical section

Concurrent accesses to
shared resources can lead to unexpected or erroneous behavior, so parts of the
program where the shared resource is accessed are protected. This protected
section is the **critical section** or **critical region**.
Typically, the critical section
accesses a shared resource, such as a data structure, a peripheral device, or a
network connection, that would not operate correctly in the context of multiple
concurrent accesses.[^wp-critical-section]

[^wp-critical-section]: [Wikipedia - Critical section](https://en.wikipedia.org/wiki/Critical_section)


[^wp-concurrent-programming]: [Wikipedia - Concurrent computing](https://en.wikipedia.org/wiki/Concurrent_computing)

## Mutual exclusion (mutex)

In computer science, **mutual exclusion** is a property of **concurrency
control**, which is instituted for the purpose of
**preventing race conditions**; it is the requirement that
one thread of execution never enter its critical section
at the same time that another concurrent thread of execution enters its own
critical section.[^wp-mutex]

[^wp-mutex]: [Wikipedia - Mutual exclustion](https://en.wikipedia.org/wiki/Mutual_exclusion)

Often the word **mutex** is used as a short form of mutual exclusion.
