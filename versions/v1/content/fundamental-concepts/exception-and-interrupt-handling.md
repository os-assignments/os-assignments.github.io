---
title: Exception and interrupt handling 
weight: 30
draft: false
---

What should be done when an exception or interrupt occurs?

## Overview

When an exception or interrupt occurs, execution transition from user mode to
kernel mode where the exception or interrupt is handled. When the exception or
interrupt has been handled execution resumes in user space.

![](/v1/images/fundamental-concepts/exception-and-interrupt-handling-v1.png)

## Details 

When an exception or interrupt occurs, execution transition from user mode to
kernel mode where the exception or interrupt is handled. In detail, the
following steps must be taken to handle an exception or interrupts. 

While entering the
kernel, the context (values of all CPU registers) of the currently executing process must first be saved to
memory. 

The kernel is now ready to handle the exception/interrupt. 

1. Determine the cause of the exception/interrupt.
2. Handle the exception/interrupt.

When the exception/interrupt have been handled the kernel performs the following
steps: 

1. Select a process to restore and resume.
2. Restore the context of the selected process.
3. Resume execution of the selected process.

![](/v1/images/fundamental-concepts/exception-and-interrupt-handling-v2.png)

## CPU context (CPU state)

At any point in time, the values of all the registers in the CPU defines the
context of the CPU. Another name used for CPU context is CPU state.

## Saving context

The exception/interrupt handler uses the same CPU as the currently executing
process. When entering the exception/interrupt
handler, the values in all CPU registers to be used by the exception/interrupt
handler must be saved to memory. The saved register values can later restored before resuming
execution of the process. 

## Determine the cause

The handler may have been invoked for a number of reasons. The handler thus
needs to determine the cause of the exception or interrupt. Information about
what caused the exception or interrupt can be stored in dedicated registers or
at predefined addresses in memory.

## Handle the exception/interrupt 

Next, the exception or interrupt needs to be serviced. For instance, if it was a
keyboard interrupt, then the key code of the keypress is obtained and stored
some where or some other appropriate action is taken. If it was an arithmetic
overflow exception, an error message may be printed or the program may be
terminated.

## Select a process to resume 

The exception/interrupt have now been handled and the kernel. 
The kernel may choose to resume the same process that was executing prior to
handling the exception/interrupt or resume execution of any other process
currently in memory. 

## Restoring context

The context of the CPU can now be restored for the chosen
process by reading and restoring all register values from memory.  

## Resume 

The process selected to be resumed must be resumed at the same point it was
stopped. The address of this instruction was saved by the machine when the
interrupt occurred, so it is simply a matter of getting this address and make
the CPU continue to execute at this address. 


