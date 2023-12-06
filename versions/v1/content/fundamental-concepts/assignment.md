---
title: Introduction to exceptions and interrupts in Mips
linkttitle: Assignment
weight: 60
assignment: mandatory
draft: false
---

<h2 class="subtitle">Mandatory assignment</h2>

## Learning outcomes

In this assignment you will study the differences between exceptions and
interrupts and how to implement a simple exception and interrupt handler. You
will also study how both exceptions and interrupts causes a transfer of control from
user mode to kernel mode and back to user mode after the exception or interrupt
have been handled by the kernel.

## Method

To study exception and interrupt handling you will load a small Mips assembly
program into the Mips simulator Mars. The program will deliberately trigger the
following exceptions:

1. Arithmetic overflow.
2. Address error.
3. Trap.

By single-stepping the program you will examine in detail what actions are taken
in order to handle each exception.
You will also study keyboard interrupts and how this can be used make the CPU do
something different while waiting for user input.
To get a fully working system you must add or change the provided code at a few
places.

## Preparations

Before you continue you must perform the following preparations.

### Learn about Mips and Mars

If you haven't done so already, go through the introduction
to [Mips and Mars](../../prerequisites/mips-and-mars).

### Install Mars on your private computer

Mars will run on any system (including Windows) as long as you
have [Java installed][java-install]. If you prefer, you may [download][download]
and install Mars on your private computer.

[java]: https://en.wikipedia.org/wiki/Java_(software_platform)

[download]: http://courses.missouristate.edu/KenVollmar/mars/download.htm


### Clone repository

If you haven't done so already, you must [clone](clone-repository) the 
[fundamental-os-concepts][repo] repository.

[repo]: https://github.com/os-assignments/fundamental-os-concepts.git

### Start Mars

If you don't have Mars installed on your private computer, you can [log in to the department Linux system][dep-linux] and start Mars from the Applications menu under Programming.

[dep-linux]: /prerequisites/department-linux-system/

![](/v1/images/mars/Gnome_Applications_Programming_Mars.png)

Mars should now start and you should see something similar to this.

![](/v1/images/mars/MARS_Startup.png)

### Open file

Open the file `mandatory/exceptions-and-interrupts.s` in Mars.


## Study the source code

Study the assembly source code of the loaded program in the built in editor
pane.

{{% notice style="tip" title="A brief overview" %}} Read the code with the intention of
getting an overview of the overall structure. Focus on **labels** and jumps to
labels. Focus on the difference between the user text segment (`.text`) and
kernel text segment (`.ktext`). You will study the details later. {{% /notice %}}

{{% notice style="tip" title="Don't edit the source" class="warning" %}}

You should not edit the source code at this stage.

{{% /notice %}}

###  User level code

After an introductory comment you find the `.text` assembler directive followed by
the label `main` which is the entry point of the user mode program. In main:

1. First an **arithmetic overflow exception** is triggered by adding `1` to the
   largest positive 32 bit [two's complement](https://en.wikipedia.org/wiki/Two's_complement) value `0x7fffffff`.
2. Next an **address error exception** is triggered by trying to load a value from an invalid memory address (address 0).
3. Finally, a **trap exception** is triggered using the `teqi` (Trap EQual Immediate) instruction.

At the end of `main` the program enters an infinite loop incrementing a counter
(register `$s0`).

### Kernel level code

The assembler directive `.ktext 0x80000180` instructs the assembler to place the
generated machine instructions in the kernel text segment starting at memory
address `0x80000180`. Here the  label `__kernel_entry_point` marks the entry point
of the exception handler (kernel). Note that this label is not needed but simply
included to make it obvious to a human reader where the exception handler
starts.

When an exception or interrupt occurs, the address of the program counter of
   the currently executing program is automatically saved to the EPC register in
   coprocessor 0 and control is transferred from user mode to kernel mode.

When entering the kernel, the kernel  must determine whether this due to an
    an exception or an interrupt.

1. First the kernel loads the value of the cause register from coprocessor 0.
2. Next the exception code is extracted from the cause register. The exception
   code will be zero for an interrupt and non-zero for an exception.
3. Using a conditional branch execution will continue at the label `__interrupt`
for an interrupt and at label `__excepetion` for an exception.

For an exception, the exception code must be further examined to distinguish
between different exceptions. For interrupts the pending interrupt bits in the
cause register is used to distinguish between different interrupts. At the end
of the kernel text segment at label `__resume`, execution is resumed in user
mode at the address saved in the EPC register in coprocessor 0.

## Adjust the run speed

Adjust the simulated run speed to 25 inst/sec or lower.

![](/v1/images/mars/mars-run-speed-25.png)

## First test run

Before you continue, **clear** both the **Mars Messages** and **Run I/O.**

{{% mars assemble %}}
Assemble the file by clicking on the  icon with the screwdriver and wrench.
{{% /mars %}}

You should see something similar to the following in the Mars Messages display pane.

``` text
Assemble: assembling /path/to/file/exceptions-and-interrupts.s

Assemble: operation completed successfully.
```

{{% mars play %}}
Click on the play icon to run the program to completion.
{{% /mars %}}

In the Run I/O display window you should see the following output.

``` text
===>      Arithmetic overflow       <===

===>      Arithmetic overflow       <===

===>      Arithmetic overflow       <===

===>      Arithmetic overflow       <===
```

The same error message is printed several times. 

{{% notice title="What is going on?" %}}

Spend some time to see if you can come up with an explanation as to why the same
error message printed over and over again.

{{% /notice %}}

{{% mars stop %}}
Click on the stop button to stop the simulation.
{{% /mars %}}

Before you continue, **clear** both the **Mars Messages** and **Run I/O.**

## Pseudo instructions

In the Execute pain the source instructions are shown in the **Source** column and
the actual instructions produced by the assembler are shown in the **Basic** column.
Note that the first source instruction `li $s0, 0x7fffffff` is a pseudo
instruction that is translated to one `lui` instruction and one `ori` instruction,
both using the `$at` (Assembler Temporary) register, i.e., register `$1`.

## Arithmetic overflow step by step

It is now time to study the execution in more detail by execute one instruction
at the time using the single step button <img class="inline" src="/v1/images/mars/icons/StepForward22.png"/>.

{{% mars assemble %}}
Assemble the file.
{{% /mars %}}

### The largest positive integer

The program starts by storing the largest 32 bit
positive [two's complement](https://en.wikipedia.org/wiki/Two's_complement)
integer in register `$s0`
using the `li` (Load Immediate) instuction. This instruction is a **pseudo
instruction** and translates to one `lui` instruction and one `ori` instruction.

{{% mars step-forward %}}

Execute the `li $s0, 0x7fffffff` instruction.

{{% /mars %}}

Now, look at the register pane.

![](/v1/images/fundamental-concepts/assignment/step-1.png)

Note that register `$at` (register number 1) have been highlighted and that the value stored in `$at`
changed from the initial value `0x00000000` to `0x7fff0000` , i.e., the upper
half of the 32 bit value `0x7fffffff` is now stored in `$at`.

{{% mars step-forward %}}
Execute the `ori $16, $1, 0x000ffff` instruction.
{{% /mars %}}

Now `$s0` (register number 4) will be highlighted in the register pane.
Register `$s0` now holds the value value `0x7fffffff` =  [32 bit binary] = `0111 1111
1111 1111 1111 1111 1111 1111`, i.e., the largest positive 32 bit
two's [two's complement](https://en.wikipedia.org/wiki/Two's_complement)
integer.

### The program counter

The program counter stores the address of the next instruction to execute.
In the register pane, look at the value of the program counter `pc`. We see that
`pc` = `0x00400008`. Also note that in the execute pane the instruction at this
address is now highlighted.

### The cause register

On the Coproc 0 tab in the register pane, look at the cause register (register
13).

![](/v1/images/fundamental-concepts/assignment/step-2.png)

The value in the cause register is currently `0x00000000`.

### Adding one

We will now try to add the value one (1) to the integer stored in `$s0`.

{{% mars step-forward %}}Execute the `addi $s1, $s0, 1` instruction.{{% /mars%}}

The program counter have now jumped from `0x00400008` to `0x80000180`, i.e.,
execution has transition to the kernel entry point. Now the status
register (register 12) is highlighted in the register pane.

![](/v1/images/fundamental-concepts/assignment/step-3.png)

Note that the cause register changed from `0x00000000` to `0x00000030` and
that EPC  have been set to
`0x00400008`, i.e., been set to the address of the `addi` instruction causing
the overflow exception.


### Step backwards and forwards

One great feature of the Mars simulator is the possibility to execute the program backwards.

{{% mars step-backward %}}
Undo the execution of  `addi $s1, $s0, 1` instruction by clicking on the undo
button.
{{% /mars%}}

Study the values of the program counter, the cause register and the EPC register.

{{% mars step-forward %}}Execute the `addi $s1, $s0, 1` instruction.{{% /mars%}}

Keep undo and redo the execution of the `addi` instruction and make sure you
understand that this addition causes a transfer of control from user mode to
kernel mode due to an overflow exception and that information about the
exception is stored in the cause register. When the exception happens, the
address of the faulty instruction is automatically saved in the EPC register.

### Transition from user mode to kernel mode

As a consequence of the overflow exception, execution has now transition from
user mode to kernel mode. Execution now continues in the `.ktext` segment at the
label `__kernel_entry_point` at address `0x80000180`.

### Get the value in the cause register

To investigate what caused the transfer from user mode to kernel mode, the
kernel must fetch the value of the cause register from coprocessor 0.

{{% mars step-forward %}}Execute the `mfc0 $k0, $13 ` instruction.{{% /mars%}}

In the register pane register `$k0` should now be highlighted with value
`0x00000030` = [binary] = `0000 0000 0000 0000 0000 0000 0011 0000`, i.e, a copy of the cause register.

### Mask all but the exception code (bits 2 - 6) to zero

The bits of the cause registers have different meaning.

![](/v1/images/mips/MIPS_Cause_register.png)

In general, we can't be sure if other bits than the exception code bits (bits
2 - 6) are set in the cause register. To set all bits but the exception code
bits (bits 2 -6) to zero, the [bitmask][bitmask] `0x0000007C` = [binary] = `0000 0000
0000 0000 0000 0000 0111 1100` is used together with bitwise and (andi).

[bitmask]: https://en.wikipedia.org/wiki/Mask_(computing)#Common_bitmask_functions

{{% mars step-forward %}}

Execute the `andi $k1, $k0, 0x00007c` instruction.

{{% /mars%}}

In the register pane, register `$k1` should now have value `0x00000030` =
[binary] = `0000 0000 0000 0000 0000 0000 0011 0000`,.

### Shift two steps to the right

To get the value of the exception code we need to shift the value in `$k1` two
steps to the right.

{{% mars step-forward %}}Execute the `srl  $k1, $k1, 2` instruction.{{% /mars%}}

Register `$k1` now hold the **exception code** =  `0x0000000c` =
[binary] = `0000 0000 0000 0000 0000 0000 0000 1100` = [decimal] = `12`.

### Interrupt or exception

The exception code is zero for an interrupt and none zero for all exceptions.
The `beqz` instruction is now used to jump to the label `__interrupt` if the
exception code in `$k1` is zero, otherwise execution continues on the next instruction.

{{% mars step-forward %}}Execute the `beqz $k1, __interrupt` instruction.{{% /mars%}}

The exception code is non zero and the branch is not taken.

### Branch depending on the exception code

Next the `beq` instruction is used to make a conditional jump to the label
`__overflow_exception` if the exception code in `$k1` is equal to `12`.

{{% mars step-forward %}}

Execute the pseudo instruction `beq $k1, 12,
__overflow_exception` by clicking **twice** on step the step forward button.

{{% /mars%}}

The branch is taken and execution continues at label `__overflow_exception`.

### Print error message

Next a "magic" Mars builtin system call is used to print the error message
    `"===> Arithmetic overflow <===\n\n"` stored as a null terminated string in
    the `.kdata` segment at label `OVERFLOW_EXCEPTION`.

{{% notice title="Mars magic" class="warning" %}}

Unfortunately the built-in system calls in Mars are implemented as part of the
underlying Mips emulator. You cannot single-step the built-in system calls to
see how they are implemented. Hence they provide us with a little magic that we
cannot study or modify.

{{% /notice %}}

{{% mars step-forward %}}

Single step **four times** execute the magic print
string system call.

{{% /mars %}}

In the **Run I/O** pane you should now see the following message.

``` console
===>      Arithmetic overflow       <===
```

Next an unconditional jump to label `__resume_from_exceptioni` is done.

{{% mars step-forward %}}Execute the `j __resume_from_exception` instruction.{{% /mars%}}

### Resume from exception

Execution now continues at the label `__resume_from_exception`. First the value
of the EPC register is now fetch from the coprocessor 0 register `$14` to the
CPU register `$k0`.

{{% mars step-forward %}}Execute the `mfc0 $k0, $14` instruction.{{% /mars%}}

Register `$k0` now have the value `0x00400008`. This is the address that was
automatically stored in EPC when the overflow exception occurred, i.e., the
address of the instruction causing the overflow exception.

Next, the value in the `$k0` register is stored back to the EPC register in
coprocessor 0.

{{% mars step-forward %}}

Execute the `mtc0 $k0, $14` instruction.

{{% /mars%}}

### Transfer control back to user mode

The exception have now been handled by the kernel. The last thing to be done is
to transfer control back to user mode using the `eret` instruction which makes
and unconditional jump to the address currently stored in EPC.

{{% mars step-forward %}}Execute the `eret` instruction.{{% /mars%}}

### Full circle

Execution now continues in user mode at the same instruction that caused the
overflow exception in the first place.

{{% mars play %}}

Click on the play button to continue execution. Don't use the single-step
button, use the play button.

{{% /mars%}}

In the Run I/O display window you should see the following output.

``` bash-session
===>      Arithmetic overflow       <===

===>      Arithmetic overflow       <===

===>      Arithmetic overflow       <===

===>      Arithmetic overflow       <===
```

The same message is printed repeatedly in the Run I/O display.

{{% mars stop %}}

Click on the stop button to stop the simulation.

{{% /mars %}}

### Solution

When resuming execution after an exception, we want to resume at the instruction
following the instruction at the address saved in EPC.
Each instruction is four bytes, hence we need to add four to EPC before
executing `eret`.

- Click on **edit** to show the source code.
- Press **Ctrl-F** and search for **todo_1**.
- Uncomment the following line to add four to the EPC value.


``` tasm
# addi $k0, $k0, 4 # TODO: Uncomment this instruction
```


{{% mars assemble %}}
Assemble the file.
{{% /mars %}}

Before you continue, **clear** both the **Mars Messages** and **Run I/O.**

{{% mars play %}}
Click on the play icon to run the program to completion.
{{% /mars %}}

In the Run I/O display window you should see the following output.

```bash-session
===>      Arithmetic overflow       <===

===>      Unhandled exception       <===

===>      Unhandled exception       <===
```

This time there is exactly one arithmetic overflow error message followed by two
messages about unhandled exceptions.

### Infinite loop

The program is now stuck in the infinite loop at label `infinite_loop`. In the
register pane you should be able to see how the value of register `$s0` is
constantly increasing.


{{% mars stop %}}

Click on the stop button to stop the simulation.

{{% /mars %}}

## Address error and trap exceptions

At label `todo_2` you must add code to:

- Branch to label `__bad_address_exception` for exception code 4.
- Branch to label `__trap__exception` for exception code 13.

{{% mars assemble %}}
Assemble the file.
{{% /mars %}}

Before you continue, **clear** both the **Mars Messages** and **Run I/O.**


{{% mars play %}}
Click on the play icon to run the program to completion.
{{% /mars %}}

In the Run I/O display window you should see the following output.

```bash-session
===>      Arithmetic overflow       <===

===>   Bad data address exception   <===

===>         Trap exception         <===
```

### Pause the simulation

This time you should not halt the simulation, instead you should pause the
simulation.

{{% mars pause %}}

Click on the pause button to pause the simulation.

{{% /mars %}}


## Keyboard interrupts

We will now make the keyboard generate an interrupt for each keypress. In order
to do this we must first setup the Mars MMIO simulator.

### Enable the Keyboard and display MMIO simulator

To make MARS simulate the memory mapped keyboard receiver (and display
transmitter) you must enable this feature.

### Open the Keyboard and Display MMIO Simulator window

From the **Tools** menu, select **Keyboard and Display MMIO Simulator**.

![](/v1/images/mars/MARS_Tools_MMIO.png)

A new window should now open.

![](/v1/images/mars/MARS_MMIO_window.png)

The lower white area of this window is the simulated keyboard.

### Connect to MIPS

To make MARS aware of the simulated memory mapped receiver (keyboard), press the
**Connect to MIPS button** in the lower left corner of the Keyboard and Display
MMIO Simulator window.

### Resume the simulation

Now you can resume the simulation.

{{% mars play %}}

Click on the play icon to resume the simulation.

{{% /mars %}}

### Infinite loop

The program is now stuck in the infinite loop at label `infinite_loop`. In the
register pane you should be able to see how the value of register `$s0` is
constantly increasing.

### Type on the simulated keyboard

Click inside the lower white area of the MMIO simulator window and type a few
characters.

![](/v1/images/fundamental-concepts/assignment/mmio-simulator-type-here.png)

Nothing happens, the program is still stuck in the infinite loop.

### Stop the simulation

Before you continue, make sure to stop the simulation.

{{% mars stop %}}

Click on the stop button to stop the simulation.

{{% /mars %}}


### Enable keyboard interrupts

At label `todo_3` you must add code to enable keyboard interrupts. The simulated
keyboard is configured by setting bits in the memory mapped transmitter control
register which appears at address `0xffff0000`.

![](/v1/images/mars/receiver-control.png)

To make the keyboard generate interrupts on keypresses, the bit 1 of receiver
control must be set to 1.

{{% mars assemble %}}

Assemble the file.

{{% /mars %}}


{{% mars play %}}

Click on the play icon to run the program to completion.

{{% /mars %}}

### Type on the simulated keyboard

Click inside the lower white area of the MMIO simulator window and type **a
single character**.

![](/v1/images/fundamental-concepts/assignment/mmio-simulator-type-here.png)

When you type a character on the simulated keyboard  a keyboard
interrupt is generated. The interrupt is handled by the kernel.

{{% notice title="Adjust the run speed" %}}

Adjust the run speed to a slower speed in order to see how the asynchronous
keyboard interrupt causes control to be transferred from the user level infinite
loop to the kernel where the interrupt is handles and then back to the user
level infinite loop.

{{% /notice %}}

{{% mars stop %}}

Click on the stop button to stop the simulation.

{{% /mars %}}

### Known bug in Mars

There is a conflict between the built-in system call 12 `read_char` and the
Keyboard and Display MMIO Simulator. 

{{% notice style="warning" title="Mars may hang" %}}
If the Keyboard and Display MMIO Simulator is open and connected, Mars 
hangs if the user types a character on the simulated keyboard while Mars
waits for input using the built-in system call 12 `read_char`.
{{% /notice %}}

### Echo

The ASCII value of the pressed key is stored in the memory mapped receiver data
register.

![](/v1/images/mars/receiver-data.png)

At label `todo_4` you must uncomment a number of insructions to load the
ASCII value from receiver control and print it to Run I/O using the Mars builtin
system cal.

{{% mars assemble %}}

Assemble the file.

{{% /mars %}}


Click on the label `__todo_4` in the labels window.

![](/v1/images/fundamental-concepts/assignment/labels-window-todo-4.png)

The instruction at label `__todo_4` is now highlighted in the Execute pane. Add
a **breakpoint** at this address by checking the checkbox in the leftmost
**Bkpt** column.

{{% mars play %}}

Click on the play icon to run the program to completion.

{{% /mars %}}

Once again, execution is stuck in the infinite loop incrementing the `$s0`
register.

### Type on the simulated keyboard

Click inside the lower white area of the MMIO simulator window and type **a
single character**.

![](/v1/images/fundamental-concepts/assignment/mmio-simulator-type-here.png)

### Execution paused at the breakpoint

When you type a character on the simulated keyboard  a keyboard
interrupt is generated. The interrupt is handled by the kernel and execution is
paused at the breakpoint.

### Single step

Continue by single stepping and try to understand how the keyboard interrupt is
handled by the kernel and execution resumes in the user mode infinite loop after
the keyboard interrupt has been handled.

Once the keyboard interrupt have been handled you should see the pressed
character printed to Run I/O display.

### Repeat

Now you can press play again, press a key on the
simulated keyboard and single step after the breakpoint. Repeat a few times to
make sure you understand how the keyboard interrupt is handled.

## Conclusions

Interrupts and exceptions are used to notify the CPU of events that needs
immediate attention during program execution. Exceptions and interrupts are events that alters the normal sequence of
instructions executed by a processor.

![](/v1/images/fundamental-concepts/exception-and-interrupt-handling-v1.png)
### Exceptions are internal and synchronous

- Exceptions are used to handle internal program errors.
- Overflow, division by zero and bad data address
  are examples of internal errors in a program.
- Another name for exception is trap. A trap (or exception) is a
  software generated interrupt.
- Exceptions are produced by the
  CPU control unit while executing instructions and are considered to be
  synchronous because the control unit issues them only after terminating the
  execution of an instruction.


### Interrupts are external and asynchronous

- Interrupts are used to notify the CPU of external events.
- Interrupts are generated by other hardware devices
  outside the CPU at arbitrary times with respect to the CPU clock signals and are
  therefore considered to be asyncronous.
- Key-presses on a keyboard might happen at any
  time. Even if a program is run multiple times with the
  same input data, the timing of the key presses will
  most likely vary.


## Preparations for the code grading

This assignments is constructed more or less as a step-by-step tutorial. The
purpose of the code grading is **not** for you (the student) to show the
teaching staff that you got something to work as required.
The focus if for you to be able to explain and discuss what you've done and what you learnt.
In preparation for the code grading:

- Make sure you understand as much as possible of all the details.
- Single step the code in Mars.
- Read all the provided comments.
- Try to understand what happens in each step.
