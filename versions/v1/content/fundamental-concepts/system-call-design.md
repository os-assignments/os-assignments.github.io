---
title: System call design
weight: 45
draft: false
---


As an example of how multiprogramming handles I/O requests we will study how to
wait for the user to input data from the keyboard. 

{{% notice style="info" title="Key-presses are asynchronous and external" %}}
A user pressing a key on a keyboard is not an internal event, nor is a
  key-press synchronous, a keypress can happen at any time independent of the
    executing program.
{{% /notice %}}


Normally the operating system is responsible for handling user input and output.
When a user program requests service from the operating system this is
implemented as system calls.

{{% notice style="info" title="System calls"%}}
System calls forms an interface between user programs and the
operating system. 
{{% /notice %}}

We will start by to study how to implement a system call that lets the user input a single
character from the keyboard. Next we will study how to implement a system call
that lets the user input a string from
the keyboard. 


## Read character system call design 

Let's sketch the design for a system call similar to the C
library function `getc` that allows a program to read a single single character typed by a human user on 
the keyboard.
The below figure shows an example where **job 1** calls the custom `getc` system
call and **job 2** executes while **job 1** waits for the user to press a key on
the keyboard. 

![](/v1/images/fundamental-concepts/getc-multiprogramming.png)

In the above figure important events are marked with a number inside a yellow
circle.

1. Job 2 is ready to run and job 1 is running. 
2. Job 1 uses the `getc` system call to read a character from the
   keyboard. 
3. The system call uses a **system call exception**  to enter the kernel. 
    * The kernel saves the context (all register values) of job 1.
    * The kernel now knows that job 1 waits for input and changes its state from
      **running** to **waiting**. 
    * The kernel restores the context of job 2.
4. The kernel resumes execution of job 2. Job 1 and changes its state from
   **ready** to **running**. 
5. The human user presses a key on the keyboard. 
6. The key-press causes a keyboard **interrupt** which is handled by the kernel. 
    * The kernel saves the context of job 2 and changes its state from
      **running** to **ready**.
    * The kernel knows that job 1 waits for the `getc` system call to complete. 
    * The kernel restores the context of job 1 and changes its state from
      **waiting** to **running**. 
    * The  ASCII value of the pressed key is made available to job 1.  
7. The kernel resumes execution of job 1.

### Multiprogramming

The result of the `getc `system call can not be obtained immediately, the kernel
must wait for the user to press a key on the keyboard. When handling the `getc`
system call, the kernel blocks the caller and make another job run until the
user presses a key on the keyboard.


## Null-terminated strings

A **null-terminated string** is a character string stored as an array containing
the characters  and terminated with a [null character](https://en.wikipedia.org/wiki/Null_character) `\0`.
[^null-terminated-string]

[^null-terminated-string]: https://en.wikipedia.org/wiki/Null-terminated_string 

## Read string system call design

We have already studied how interrupts can be used to wait for a single keypress
from a human user. How can we design a system call for inputting a string?

*   To input a string we simply needs to wait for each keyboard interrupt and
    save the characters in an array (buffer).
*   When the user presses enter (or there is no more space in the buffer) the
    buffer is terminated with null.
    
### Input buffer

Where should the input buffer be allocated? In user space or in kernel space?

### Memory safety

*   It is not desirable for a user program to be able to read arbitrary data
    stored in the kernel.
*   A user program could (possibly by mistake) corrupt the kernel by
    writing data outside the input buffer. This is called [buffer overflow](https://en.wikipedia.org/wiki/Buffer_overflow).

To enforce memory safety user programs are not allowed to access (read or write)
kernel data but the kernel is allowed to read and write data in user space.

### Buffer pointer

The kernel must know the address (pointer) to the input buffer in user space. 

### Buffer size

The kernel must know the size of the input buffer and decide what to do when the
buffer is full.

### Read string system call example

To better understand the details of how the read string system call can be
implemented we will study a small example with two jobs. In this example, job 1 allocates a
3 element input buffer and uses a system call to request the operating
system to wait (using interrupts) for the user to fill this buffer with two
characters and terminate the buffer with null. Until the input buffer is full
the operating system (kernel) let job 2 execute between keyboard
interrupts. 

![](/v1/images/fundamental-concepts/read-string-multiprogramming.png)

## System call convention

A common pattern for system calls and other library functions is for the caller
to allocate a struct or array in user space. The caller pass a pointer to the struct or array as an
argument to the system call or library function call. The system call or library
function writes result data to the struct or array in user space using the
provided pointer. 

An alternative to the above patterns would be to return data (structure or array
elements) on the stack. So
why are pointers (call by reference) used instead of returning on the stack? 

- [Why do many functions that return structures in C, actually return pointers to structures?][so]  

[so]: https://softwareengineering.stackexchange.com/questions/359408/why-do-many-functions-that-return-structures-in-c-actually-return-pointers-to-s
