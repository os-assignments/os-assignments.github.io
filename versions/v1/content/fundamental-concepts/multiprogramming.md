---
title: Multiprogramming
weight: 40
draft: false
---

In general, I/O operations does not make use of the CPU but are handled by external devices.
Compared to the CPU, I/O operations are very slow and while waiting for I/O the CPU
is idle doing nothing. To overcome this problem **multiprogramming** was
invented. 

## Job

In systems using multiprogramming a program loaded to memory and ready
to execute is called a **job**.

## Execute another job while waiting for I/O

The simple idea is to always have one job execute on the CPU by changing job when an I/O request is made. 

## States

In a multiprogramming system, a job can be in one of three states. 

Running
: The job is currently executing on the CPU. At any time, at most one job can be in this state.

Ready
:  The job is ready to run but currently not selected to do so. 

Waiting
: The job is blocked from running on the CPU while waiting for an I/O request to be
  completed. 

## State transitions

In a multiprogramming system, a the following state transitions are possible. 

| From       | To        | Description                                                                                                                   |
| :--------: | :-------: | -------------                                                                                                                 |
| Running    | Waiting   | When a running job requests I/O, the job changes state from running to waiting.                                               |
| Running    | Ready     | When an I/O requests completes, the running job changes state from running to ready.                                          |
| Waiting    | Ready     | When an I/O requests completes, the job waiting for the request to complete  changes state from waiting to ready.             |
| Ready      | Running   | When an I/O requests completes, one of the ready jobs are selected to run on the CPU and changes state from ready to running. |


The problem is, how will the system know when an I/O request is completed?

## Interrupts

To implement multiprogramming **interrupts** are used to notify the system of
important events such as the completion of an I/O request. 

## Step by step

In a multiprogramming system, several jobs are kept in memory at the same time.
Initially, all jobs are int the ready state.

![](/v1/images/module-1/multiprogramming-1.png?width=500px)

One of the ready jobs is selected to execute on the CPU and changes state from
ready to running. In this example, job 1 is selected to execute. 

![](/v1/images/module-1/multiprogramming-2.png?width=500px)

Eventually, the running job makes a request for I/O and the state  changes from
running to waiting.

![](/v1/images/module-1/multiprogramming-3.png?width=500px)

Instead of idle waiting for the I/O request to complete, one of the ready jobs is
selected to execute on the CPU and have its state change from ready to running.
In this example job 3 is selected to execute. 

![](/v1/images/module-1/multiprogramming-4.png?width=500px)

Eventually the the I/O request  job 1 is waiting for will complete and the CPU will be
notified by an interrupt. In this example, job 1 was waiting for a keypress on
the keyboard. 

![](/v1/images/module-1/multiprogramming-5.png?width=500px)

The state of the waiting job (job 1) will change
from waiting to ready.

![](/v1/images/module-1/multiprogramming-6.png?width=500px)

