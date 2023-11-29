---
draft: false
---

# System overview 

You will be provided with MIPS assembly skeleton source code for a system
implementing a simplified version of
[multiprogramming](waiting-for-keyboard-input/#multiprogramming).
On this page you find an overview of the system design.

An overview of the system is shown in the below figure. 

![](/images/MARS_multiprogramming_system_overview.png)

In short, the system will have two jobs taking turn executing on the CPU. The
kernel will use multiprogramming to switch jobs only when a job requests I/O. 

In the remaining sections details of the system  will be explained. 

# Two non-terminating jobs

To make the system as simple as possible, the kernel supports only two
non-terminating jobs. Only one job can execute on the CPU at the time.

On startup of the system (boot) one job is selected to execute (**running**) and
the other is marked as **ready** to run.

# Job id (jid)

The two jobs are identified by job id (`jid`) numbers `0` and `1` respectively.

# No stack and no subroutines

To keep the system simple the jobs cannot use the stack nor can they make
subroutine calls. 

# No memory protection 

MARS does not support any form of memory protection. As a consequence there will be no
memory protection between user space and kernel, nor will there be any memory
protection between jobs. 

The two jobs:

* execute in the same memory space
* share the same data segment.

# Only one job can request I/O

The whole purpose of multiprogramming is to maximize the usage of the CPU. A job
executes on the CPU until requesting I/O. When requesting I/O the job is blocked
waiting for the I/O request to complete. While waiting the other job executes
on the CPU.

In this simplified system, only one of the two non-terminating jobs can request
I/O. While a job is waiting for the I/O request to complete the other job
executes on the CPU.

# Kernel

In this system the kernel is responsible for handling all exceptions and
interrupts. The kernel will also be responsible for managing the the execution
of the two jobs. 

# Kernel stack

In order for the kernel to be able to use subroutines, the the kernel will
allocate a stack. 


# Job context

When a job executes it uses the CPU registers. At any time, the values of all
the registers is called the **context** of the running job. 

In this simplified system each job is only allowed to use the following five
registers: `$pc` (program counter), `$v0`, `$a0`, `$s0` and `$at` (should only
be used by pseudo instructions).

Each register is four byte, hence 5*4 = 20 bytes of storage is needed to store
the context. The contents of the registers are saved in the following order in each job
context. 

![](/images/MARS_multiprogramming_job_contexts.png)

## Addressing the context

If the address to a context is in register `$t0`, the address to each of the
fields in the context is shown in the column **Assembly notation** in the
following table. 

| Field  | Offset | Address         | Assembly notation |
| ------ | ------ | --------------- |                   |
| `$pc`  | `0`    | `$t0 + 0`       | `0($t0)`          |
| `$v0`  | `4`    | `$t0 + 4`       | `4($t0)`          |
| `$a0`  | `8`    | `$t0 + 8`       | `8($t0)`          |
| `$s0`  | `12`   | `$t0 + 12`      | `12($t0)`         |


### Examples

In the following examples, the address to a context is in register `$t0`.

``` 
sw $t1,  8($t0)  # Store $t1 to the $a0 field in the context. 

lw $t2, 12($t0)  # Load the $s0 field from the context into $t2. 
```

# Context array

To store the contexts of the two jobs, a two element array is used. The array is
indexed by job id (0 or 1). Each element of the array is a **pointer** to
storage allocated for the context.

![](/images/MARS_multiprogramming_context_array.png)

## Addressing the context array

The two element context array is indexed by job id (`0` or `1`). Each element of
the context array is a pointer to a job context, i.e., a four byte address to a
job context.


If `ca` is the address of the context array and `jid` the job id, the following table shows
how to calculate the address to the context pointer the job with id `jid`.

| Job id (`jid`) | Offset to context pointer | Address of context pointer |
| -------        | -----                     | ---                        |
| `0`            | `0 == jid * 4`            | `ca + jid * 4`             |
| `1`            | `4 == jid * 4`            | `ca + jid * 4`             |

### Example

Multiplying with 4 is equivalent to shift left two bits. In MIPS assembly the
`sll` (Shif Left Logical) instruction is used to bit shift to the left. 

If the context array is stored at label `__context_array` and the job id (`jid`)
the of the running job is stored at label `__running` the following example shows how to
get the address to the context of the running job. 

``` 
    # Get job id (jid) of the running job. 
	
	lw $t0, __running 
	
	# Offset to context pointer (0 or 4)  = jid * 4.
	
	sll $t1, $t0, 2   # jid * 4
	
	# Pointer to context.
	
	lw $t3, __context_array($t1)
```

Now `$3` contains the address of the running jobs context and can be used to
access fields within the context as already described in the
section [addressing the context](#addressing-the-context). 


# Save context

When entering the kernel, the context of the running job is saved. i.e., the
contents of `$pc`, `$v0`, `$a0`, `$s0` and `$at` are saved to memory. 
12($t0)
# Restore context

When the kernel resume the execution of a job, the job context is first
restored, i.e., the values of `$pc`, `$v0`, `$a0`, `$s0` and `$at` that
previously been saved to memory are read from memory and restored. 
12($t0)
# System calls built-in to MARS are magic

Previously you have used a number
of [system calls built in to MARS][mars-system-calls], for example to print
strings and integers. 

Unfortunately the built-in system calls in MARS are implemented as part of the
underlying MIPS emulator. You cannot single-step the built-in system calls to
see how they are implemented. Hence they provide us with a little magic that we
cannot study or modify. 

[mars-system-calls]: http://courses.missouristate.edu/KenVollmar/mars/Help/SyscallHelp.html
12($t0)
# The syscall instruction

Before calling one of the built-in system calls, the system call code is placed
in `$v0`. Next, to perform the system call, the `syscall` instruction is used
to cause system call exception (exception code) that causes control to be
transferred from user space to kernel space where the system call is handled.
The  kernel investigates the value in `$v0` to see what system call the user
requested. 

In MARS the built-in system calls are handled by the simulator and not by the
kernel (exception and interrupt handler) code that you can modify. Unfortunately
this makes it impossible to provide custom implementations of system calls using
the `syscall` instruction.
12($t0)
# Two custom system calls

After all, this is a course about operating systems and we cannot be satisfied
with the magic provided by the system calls built in to MARS. It is now time to see how
we can implement our own custom system calls. 

The kernel will implement the following two custom system calls. 

| System call code | Name     | Description                                | Return value                     |
|:--------------:| ------   | ------------------------------------------ | -----------------                |
| `0`              | `getjid` | Get job id of the caller                   | `$a0` - job id of the caller     |
| `12`             | `getc`   | Read a single character from the keyboard  | `$v0` - ASCII code of pressed key |

# The teqi instruction

The `teqi` (Trap EQual Immediate) instruction is used to conditionally trigger a
trap exception (exception code 13) that automatically transfers control to the kernel.

``` 
teqi rs, imm
```

The  `teqi` instruction will cause a trap exception (code 13) if the value in
register `rs` is equal to the immediate constant `imm`.

**Example usage: **To trigger a trap exception we can compare `$zero` (allways
zero) with zero.

``` 
teqi $zero, 0
```

Instead of using the `syscall` instruction we can use `teqi $zero, 0` when we
want to perform one of the custom system calls.
12($t0)
# States and state transitions

At any time, each of the two non-terminating jobs job can be in one of the
following three states. 

| State   | Description                                                                                                                               |
| ------- | ----------------------------------                                                                                                        |
| Running | The job currently executing on the CPU.                                                                                                   |
| Ready   | The job is ready to run but currently not selected to do so.                                                                              |
| Waiting | The job is blocked from running waiting for the custom `getc` system call to be completed, that is, waiting for the human user to press a key on the keyboard.  |

12($t0)
# Calling the custom getjid system call

An example showing how to use the custom `getjid` system call to get the job id
of the caller:

``` 
li i $v0, 0
teqi $zero, 0

# On success $a0 will now contain the job id (jid) of the caller. 
```

The below figure shows an example where **job 1** calls the custom `getjid`
system call. 

![](/images/getjid_multiprogramming.png)

In the above figure important events are marked with a number inside a yellow
circle. 

1. Job 2 is ready to run and job 1 is running. 
2. Job 1 uses a the custom `getc` system call to read a character from the
   keyboard. 
   
3. The system call uses a **trap exception**  to enter the kernel. 
   
    * The kernel saves the context (all register values) of job 1.
    * Besides `$k0` an `$k1` the kernel can now safely use any of the registers saved to
     the job 1 context.
     
    * The kernel knows that the running job has id 1. 
   
    * The kernel restores the context of job 1.
   
    * The kernel restores the context of job 1. 
   
    * The kernel put the value 1 (the job id) in register `$a0`.
   
4. The kernel resumes execution of job 1. 
12($t0)
## A non-blocking system call

Note that the custom `getjid` system call is an example of a **non-blocking**
system call. The result of the system call can be obtained quickly and the
kernel can resume the execution right away. 
s
# Calling the custom getc system call

An example showing how to use the custom `getc` system call to read a character
from the keyboard. 

``` 
li   $v0, 12
teqi $zero, 0

# On succes $v0 will now contain the ASCII code of the pressed key. 
```

The below figure shows an example where **job 1** calls the custom `getc` system
call and **job 2** executes while **job 1** waits for the user to press a key on
the keyboard. 

![](/images/getc_multiprogramming.png)

In the above figure important events are marked with a number inside a yellow
circle.

1. Job 2 is ready to run and job 1 is running. 
2. Job 1 uses the custom `getc` system call to read a character from the
   keyboard. 

3. The system call uses a **trap exception**  to enter the kernel. 
   
    * The kernel saves the context (all register values) of job 1.
    * The kernel now knows that job 1 waits for input and changes its state from
      **running** to **waiting**. 
    * The kernel restores the context of job 2.

4. The kernel resumes execution of job 2. Job 1 and changes its state from
   **ready** to **running**. 

5. The human user presses a key on the keyboard. 

6. The key-press causes a keyboard **interrupt** which is handled by the kernel. 
    
    * The kernel knows that job 1 waits for the `getc` system call to complete. 
    * The kernel saves the context of job 2 and changes its state from
      **running** to **ready**.
    * The kernel restores the context of job 1 and changes its state from
      **waiting** to **running**. 
    * The  ASCII value of the pressed key is placed in `$v0`. 
7. The kernel resumes execution of job 1 with the ASCII value of the pressed key
in `$v0`.

## A blocking system call

Note that the custom `getc` system call is an example of a **blocking**
system call. The result of the system call can not be obtained quickly, the
kernel must wait for the user to press a key on the keyboard. The kernel blocks
the caller and make another job run until the user presses a key on the
keyboard. 

