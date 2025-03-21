---
linktitle: Higher grade assignment
title: Multiprogramming and custom system calls in Mips
assignment: higher-grade
points: 3
weight: 70
draft: false
---

<h2 class="subtitle">Optional assignment for higher grade (3 points)</h2>

## The Mars built-in system calls

Previously you have used a number
of [system calls built in to MARS][mars-system-calls], for example to print
strings and integers. Before calling any of the Mars built-in system calls, the system call code is placed
in `$v0`. Next, the  `syscall` instruction is used to invoke the system call. 

[mars-system-calls]: http://courses.missouristate.edu/KenVollmar/mars/Help/SyscallHelp.html

In a real Mips system, the `syscall` instruction triggers a system call
exception (exception code 8) that causes control to be transferred from user
space to kernel space where the system call is handled. The kernel investigates
the value in `$v0` to determine which specific system call the user requested.

In Mars the built-in system calls are handled by the simulator and not by the
kernel (exception and interrupt handler) code that you can study and modify. Unfortunately
this makes it impossible to provide custom implementations of system calls using
the `syscall` instruction. 

## No more magic

After all, this is a course about operating systems and we cannot be satisfied
with the magic provided by the system calls built in to Mars. It is now time to
implement our own custom system calls from scratch. 

## Timer interrupts

Unfortunately, Mars does not support timer interrupts. This makes it impossible
for the kernel to to use a timer to swap between running jobs at regular
intervals. 

## Multiprogramming

For the higher grade assignment you should implement a simplified version
of [multiprogramming](multiprogramming).

## System overview 

An overview of the system is shown in the below figure. 

![](/v1/images/fundamental-concepts/higher-grade/so.png?width=700px)

For simplicity there will be exactly two jobs in the system. The kernel will implement three custom system calls: `getjid`, `getc` and
`gets`. The kernel will use multiprogramming to switch job only when a job
requests I/O using the `getc` or `gets` system call. 

## Two non-terminating jobs

To make the system as simple as possible, the kernel only need to support two
non-terminating jobs. Only one job can execute on the CPU at the time.

### Job id (jid)

The two jobs are identified by job id (`jid`) numbers `0` and `1` respectively.

### No stack and no subroutines

To keep the system simple the jobs cannot use the stack nor can they make
subroutine calls. 

### No memory protection 

Mars does not support any form of memory protection. As a consequence there will be no
memory protection between user space and kernel, nor will there be any memory
protection between jobs. Thetwo jobs:

* execute in the same memory space
* share the same data segment.

### Only one job can request I/O

The whole purpose of multiprogramming is to maximize the usage of the CPU. A job
executes on the CPU until requesting I/O. When requesting I/O the job is blocked
waiting for the I/O request to complete. While waiting the other job executes
on the CPU.

In this simplified system, only one of the two non-terminating jobs can request
I/O. While a job is waiting for the I/O request to complete the other job
executes on the CPU.

## States

At any time, each of the two non-terminating jobs job can be in one of the
following three states. 

Running
: The job is selected to execute on the CPU.

Ready
: The job is not selected to execute on the CPU but ready to do so if selected. 

Waiting
: The job has made a request for I/O using either `getc` or `gets` and s blocked
from executing until the the request have been completed.

On startup of the system (boot) one job is selected to execute (**running**) and
the other is marked as **ready** to run.

## State transitions 

In this simplified version of a  a multiprogramming system with only two jobs,
the following state transitions are possible. 

|  From   |   To    | Description                                                                                                        |
| :-----: | :-----: | ------------------------------------------------------------------------------------------------------------------ |
| Running | Waiting | When the running job requests I/O, the job changes state from running to waiting.                                  |                      |
| Running |  Ready  | When an I/O requests completes, the running job changes state from running to ready.                               |
| Waiting | Running | When an I/O requests completes, the job waiting for the request to complete changes state from waiting to running. |

In the below diagram a sequence of five events shows how the two jobs change
 states.
 
![](/v1/images/fundamental-concepts/higher-grade/example-state-transitions.png)

## Kernel

In this system the kernel is responsible for handling all exceptions and
interrupts. The kernel will also be responsible for managing the execution
of the two jobs. 

## Kernel stack

In order for the kernel to be able to use subroutines, the the kernel will
allocate a stack. 

## Job context

When a job executes it uses the CPU registers. At any time, the values of all
the registers is called the **context** of the running job. 

In this simplified system each job is only allowed to use the following six
registers: `$pc` (program counter), `$v0`, `$a0`, `$a1`, `$s0` and `$at` (should only
be used by pseudo instructions).

Each register is four byte, hence 6*4 = 24 bytes of storage is needed to store
the context. The contents of the registers are saved in the following order in each job
context. 

![](/v1/images/fundamental-concepts/higher-grade/contexts.png?width=500px)

### Addressing the context

If the address to a context is in register `$t0`, the address to each of the
fields in the context is shown in the column **Assembly notation** in the
following table. 


| Field | Offset | Address    | Assembly notation |
| :---: | :----: | ---------- | ----------------- |
| `$pc` |  `0`   | `$t0 + 0`  | `0($t0)`          |
| `$v0` |  `4`   | `$t0 + 4`  | `4($t0)`          |
| `$a0` |  `8`   | `$t0 + 8`  | `8($t0)`          |
| `$a1` |  `12`  | `$t0 + 12` | `12($t0)`         |
| `$s0` |  `16`  | `$t0 + 16` | `16($t0)`         |


### Examples

In the following examples, the address to a context is in register `$t0`.

``` tasm
sw $t1,  8($t0)  # Store $t1 to the $a0 field in the context. 

lw $t2, 16($t0)  # Load the $s0 field from the context into $t2. 
```

## Context array

To store the contexts of the two jobs, a two element array is used. The array is
indexed by job id (0 or 1). Each element of the array is a **pointer** to
storage allocated for the context.

![](/v1/images/fundamental-concepts/higher-grade/context-array.png?width=600px)

### Addressing the context array

The two element context array is indexed by job id (`0` or `1`). Each element of
the context array is a pointer to a job context, i.e., a four byte address to a
job context.


If `ca` is the address of the context array and `jid` the job id, the following table shows
how to calculate the address to the context pointer the job with id `jid`.

| Job id (`jid`) | Offset to context pointer | Address of context pointer |
| :------------: | ------------------------- | -------------------------- |
|      `0`       | `0 == jid * 4`            | `ca + jid * 4`             |
|      `1`       | `4 == jid * 4`            | `ca + jid * 4`             |

### Example

Multiplying with 4 is equivalent to shift left two bits. In Mips assembly the
`sll` (Shif Left Logical) instruction is used to bit shift to the left. 

If the context array is stored at label `__context_array` and the job id (`jid`)
the of the running job is stored at label `__running` the following example shows how to
get the address to the context of the running job. 

``` tasm
# Get job id (jid) of the running job. 
	
lw $t0, __running 
	
# Offset to context pointer (0 or 4)  = jid * 4.
	
sll $t1, $t0, 2   # jid * 4
	
#  Pointer to context.
	
lw $t3, __context_array($t1)
```

Now `$3` contains the address of the running jobs context and can be used to
access fields within the context as described in the
section [addressing the context](#addressing-the-context). 

## Save context

When entering the kernel, the context of the running job must be saved. i.e., the
contents of `$pc`, `$v0`, `$a0`, `$a1`, `$s0` and `$at` must be saved to memory.

## Restore context

When the kernel resume the execution of a job, the job context is first
restored, i.e., the values of `$pc`, `$v0`, `$a0`, `$a1`,  `$s0` and `$at` that
previously been saved to memory are read from memory and restored. 

## Custom system calls

The kernel will implement the following system calls. 

| System call code | Name     | Description                                   | Arguments                       | Return value                      |
| :--------------: | -------- | --------------------------------------------- | ------------------------------- | --------------------------------- |
|       `0`        | `getjid` | Get job id of the caller                      |                                 | `$a0` - job id of the caller      |
|       `12`       | `getc`   | Read a single character from the keyboard     |                                 | `$v0` - ASCII code of pressed key |
|       `8`        | `gets`   | Read a string of characters from the keyboard | $a0 = buffer, $a1 = buffer size |                                   |

### The teqi instruction

The `teqi` (Trap EQual Immediate) instruction is used to conditionally trigger a
trap exception (exception code 13) that automatically transfers control to the kernel.

``` tasm 
teqi rs, imm
```

The  `teqi` instruction will cause a trap exception (exception code 13) if the value in
register `rs` is equal to the immediate constant `imm`.

**Example usage: **To trigger a trap exception we can compare `$zero` (allways
zero) with zero.

``` tasm 
teqi $zero, 0
```

Instead of using the `syscall` instruction we use `teqi $zero, 0` when we
want to perform one of the custom system calls.


### Calling the custom getjid system call

An example showing how to use the custom `getjid` system call to get the job id
of the caller:

``` tasm 
li   $v0, 0
teqi $zero, 0

# On success $a0 will now contain the job id (jid) of the caller. 
```

The below figure shows an example where **job 1** calls the custom `getjid`
system call. 

![](/v1/images/fundamental-concepts/higher-grade/getjid_multiprogramming.png)

In the above figure important events are marked with a number inside a yellow
circle. 

1. Job 1 is ready to run and job 0 is running. 
2. Job 0 uses a the custom `getc` system call to read a character from the
   keyboard. 
3. The system call uses a **trap exception**  to enter the kernel. 
    * The kernel saves the context (all register values) of job 0.
    * Besides `$k0` an `$k1` the kernel can now safely use any of the registers saved to
     the job 1 context.
    * The kernel knows that the running job has id 0. 
    * The kernel restores the context of job 0.
    * The kernel put the value 0 (the job id) in register `$a0`.
4. The kernel resumes execution of job 0. 

### Calling the custom getc system call

An example showing how to use the custom `getc` system call to read a character
from the keyboard. 

``` tasm 
li   $v0, 12
teqi $zero, 0

# On succes $v0 will now contain the ASCII code of the pressed key. 
```

The below figure shows an example where **job 1** calls the custom `getc` system
call and **job 2** executes while **job 1** waits for the user to press a key on
the keyboard. 

![](/v1/images/fundamental-concepts/higher-grade/getc_multiprogramming.png)

In the above figure important events are marked with a number inside a yellow
circle.

1. Job 1 is ready to run and job 0 is running. 
2. Job 0 uses the custom `getc` system call to read a character from the
   keyboard. 
3. The system call uses a **trap exception**  to enter the kernel. 
    * The kernel saves the context (all register values) of job 0.
    * The kernel now knows that job 0 waits for input and changes its state from
      **running** to **waiting**. 
    * The kernel restores the context of job 1.
4. The kernel resumes execution of job 1 and changes its state from
   **ready** to **running**. 
5. The human user presses a key on the keyboard. 
6. The key-press causes a keyboard **interrupt** which is handled by the kernel. 
    * The kernel knows from earlier that job 0 waits for the `getc` system call to complete. 
    * The kernel saves the context of job 1 and changes its state from
      **running** to **ready**.
    * The kernel restores the context of job 0 and changes its state from
      **waiting** to **running**. 
    * The  ASCII value of the pressed key is placed in `$v0`. 
7. The kernel resumes execution of job 0 with the ASCII value of the pressed key
in `$v0`.

## Known bug in Mars

There is a conflict between the built-in system call 12 `read_char` and the
Keyboard and Display MMIO Simulator. 

{{% notice style="warning" title="Mars may hang" %}}
If the Keyboard and Display MMIO Simulator is open and connected, Mars 
hangs if the user types a character on the simulated keyboard while Mars
waits for input using the built-in system call 12 `read_char`.
{{% /notice %}}


## Clone repository

If you haven't done so already, you must [clone](clone-repository) the 
[fundamental-os-concepts][repo] repository.

[repo]: https://github.com/os-assignments/fundamental-os-concepts.git


## Provided code

You don't have to start from scratch but can use the provided 
`higher-grade/multiprogramming.s` as a starting point. 

## Higher grade points (max 3)


For 1 point your system must have working implementations of the custom `getjid` and
`getc` system calls. Follow the labels `TODO_1`,
`TODO_2`, ..., `TODO_8` in the provided `multiprogramming.s` file to guide you
step by step.


For 3 points, in addition to the 1 point requirements above, your system must have a working implementation of the custom `gets`
system call. You must also provide a user level job with a few test cases
showing that `gets` works for different buffer sizes. 




