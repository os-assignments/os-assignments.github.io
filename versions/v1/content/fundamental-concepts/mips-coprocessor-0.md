---
linktitle: Coprocessor 0
title: Mips coprocessor 0
weight: 50
draft: false
---

A MIPS processor consists of an integer processing unit (the CPU) and a
collection of coprocessors that perform ancillary tasks or operate on other
types of data such as floating-point numbers.

![](/v1/images/mips/MIPS_CPU_FPU_Coprocessor_0.png)

Integer arithmetic and logical operations are executed directly by the CPU.
Floating point operations are executed by Coprocessor 1. Coprocessor 0 is used
do manage exceptions and interrupts.

Normal user level code doesn't have access Coprocessor 0, but interrupt and
exception aware code has to use it. Coprocessor 0 has several registers which
controls exceptions and interrupts. 

## Status (register 12)

The coprocess 0 status register is a read-write register  used to enable or disable various types of interrupts. 

![](/v1/images/mips/MIPS_Status_register.png)

### Interrupt enable

If the **interrupt enable** bit is 1, interrupts are allowed. If it is 0, they are disabled.


### Exception level 

The **exception level** bit is normally 0, but is set to 1 after an exception
occurs. When this bit is 1, interrupts are disabled and the EPC is not updated
if another exception occurs. This bit prevents an exception handler from being
disturbed by an interrupt or exception, but it should be reset when the handler
finishes. 

### User mode 

The **user mode** bit is 0 if the processor is running in kernel mode and 1 if it is
running in user mode. 

{{% notice style="warning" title="User mode fixed to 1 in Mars and Spim" %}}
Neither Mars nor Spim implements kernel mode and fixes the user mode bit to 1. 
{{% /notice %}}


### Interrupt mask

The 8 bit **interrupt mask** 8 field contains one bit for each of the 6 hardware level interrupts and the 2 
software level interrupts. A mask bit set to 1 allows interrupts at that level
to interrupt the processor. A mask bit set to 0 disables interrupts at that
level. 

<table>
<thead>
<tr>
<th rowspan=2 class="center">Hardware interrupt level</th>
<th rowspan=2>Used for</th>
<th colspan=2 class="center no-border-b">Mars</th>
<th colspan=2 class="center no-border-b"/>Spim</th>
</tr>
<tr>
<th class="center">Bit nr.</th>
<th>Bit mask</th>
<th class="center">Bit nr.</th>
<th>Bit mask</th>
<tr>
</thead>

<tbody>

<tr>
<td class="center">0</td>
<td>Transmitter (terminal output)</td>
<td class="center">9</td>
<td class="tt"> 0x00000200</td>
<td class="center">10</td>
<td class="tt">0x00000400</td>
</tr>

<tr>
<td class="center">1</td>
<td>Receiver (keyboard interrupt)</td>
<td class="center">8</td>
<td class="tt"> 0x00000100</td>
<td class="center">11</td>
<td class="tt">0x00000800</td>
</tr>

<tr>
<td class="center">5</td>
<td>Timer</td>
<td colspan=2 class="center">Not supported</td>
<td class="center">15</td>
<td class="tt">0x00008000</td>
</tr>




</tbody>
</table>


### Startup value

On startup the status register has the following value. 

| Register   | MARS         | SPIM         |
| :--------: | :-------:    | :-----:      |
| Status     | <tt>0x0000ff11</tt> | <tt>0x3000ff10</tt> |

To see which bits are set in the status register on startup we must translate the hexadecimal startup value to binary. 

{{% notice title="From hexadecimal to binary" %}}
When translating from hexadecimal to binary we do this by translating each hexadecimal digit to a four bit binary value. 

- `0x0` = [binary] = `0000`
- `0x1` = [binary] = `0001`
- `0xf` = [binary] = `1111`

Next, we replace each hexadecimal digit with the corresponding four bit binary
number.
{{% /notice %}}

Mars initializes the status register in coprocessor 0 to `0x0000ff11` = [binary] = `0000 0000 0000 0000 1111 1111 0001
0001`. 

<img src="/v1/images/fundamental-concepts/mars-status-register-startup.png" />

On startup: 

- Interrupts are enabled.
- User mode is set to 1.
- All interrupt mask bits are set to 1.

## Cause (register 13)

The Cause register, is a mostly read-only register whose value is set by the
system when an interrupt or exception occurs. It specifies what kind of
interrupt or exception just happened.

![](/v1/images/mips/MIPS_Cause_register.png)

When an exception or interrupt occur, a code is stored in the cause register as
a 5 bit value (bits 2-6). This field is commonly referred to as the exception
code although it is used for both exceptions and interrupts. 

| Exception code | 	Name | 	Cause of exception                              |
|:--------------:| ------   | ------------------                                  |
|              0 | Int      | Interrupt (hardware)                                |
|              4 | AdEL     | Address Error exception (Load or instruction fetch) |
|              5 | AdES     | Address Error exception (Store)                     |
|              6 | IBE      | Instruction fetch Buss Error                        |
|              7 | DBE      | Data load or store Buss Error                       |
|              8 | Sys      | Syscall exception                                   |
|              9 | Bp       | Breakpoint exception                                |
|             10 | RI       | Reversed Instruction exception                      |
|             11 | CpU      | Coprocessor Unimplemented                           |
|             12 | Ov       | Arithmetic Overflow exception                       |
|             13 | Tr       | Trap                                                |
|             14 | FPE      | 	Floating Point Exception                        |

### EPC (register 14) 

Exception Program Counter (EPC) register. When an interrupt or exception occurs,
the address of the currently executing  instruction is copied from the Program
Counter (PC) to EPC. This is the address that your handler jumps back to when it
finishes.

### Timer (register 9 and 11)

In SPIM, a timer is simulated with two more coprocessor registers: **Count**
(register 9), whose value is continuously incremented by the hardware, and
**Compare** (register 11), whose value can be set. When Count and Compare are
equal, an interrupt is raised, at Cause register bit 15.

To schedule a timer interrupt, a fixed amount called the time slice (quantum) is
stored in the Compare register. The smaller the time slice, the greater the
frequency of timer interrupts.

{{% notice style="warning" title="No timer in Mars" class="warning" %}}
Timer interrupts are yet not implemented in MARS.
{{% /notice %}}

## Accessing registers in Coprocessor 0

To access a register in Coprocessor the register value must be transferred from
Coprocessor 0 to one of the regular CPU registers. To update one of the
registers in Coprocessor 0 a value in one of the of the regular CPU register in
the CPU must be transferred to to one of the Coprocessor 0 registers.

If you want to modify a value in a Coprocessor 0 register, you need to move the
register's value to a general- purpose register with `mfc0`, modify the value
there, and move the changed value back with `mtc0`.

### mfc0

The `mfc0` (Move From Coprocessor 0) instruction moves a value from a Coprocessor 0 register to a general-purpose register. 

Example: 

``` tasm 
mfc0 $t5, $13  # Copy $13 (cause) from coprocessor 0 to $t5.
```

### mtc0

The `mtc0` (Move To Coprocessor 0) instruction moves a value from a
general-purpose register to a Coprocessor 0 register. 

Example:

``` tasm 
mtc0 $v0, $12  # Copy $v0 to $12 (status) in coprocessor 0.
```

## References

[Assemblers, Linkers, and the SPIM Simulator](http://spimsimulator.sourceforge.net/HP_AppA.pdf)
