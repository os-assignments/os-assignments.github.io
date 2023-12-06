---
title: Signals
assignment: mandatory
weight: 30
draft: false
---

<h2 class="subtitle">Mandatory assignment</h2>

Signals are a limited form of inter-process communication (IPC), typically used
in [Unix][wp-unix], [Unix-like][wp-unix-like], and other [POSIX][wp-posix]-compliant operating systems. [^wp-signal-ipc]
A signal is used to notify a process of an synchronous or asynchronous event.

[wp-unix]: https://en.wikipedia.org/wiki/Unix
[wp-unix-like]: https://en.wikipedia.org/wiki/Unix-like

When a signal is sent, the operating system interrupts the target process'
normal flow of execution to deliver the signal. If the process has previously registered a
signal handler, that routine is executed. Otherwise, the default signal handler
is executed. [^wp-signal-ipc]

Each signal is represented by an integer value. Instead of using the numeric
values directly, the named constants defined in [signals.h][signals.h]
should be used.

<!-- process' https://ell.stackexchange.com/a/6320 -->

[wp-posix]: https://en.wikipedia.org/wiki/POSIX

[^wp-signal-ipc]: <https://en.wikipedia.org/wiki/Signal_(IPC)>

## Clone repository

If you haven't done so already, you must [clone](clone-repository) the 
[processes-and-ipc][repo] repository.

[repo]: https://github.com/os-assignments/processes-and-ipc.git

## Open file

Open the file `mandatory/src/signals.c` in
the [source code editor][source-code-editor] of your choice.

[source-code-editor]: https://en.wikipedia.org/wiki/Source_code_editor

## Study the source code

Study the C  source code.

### Header files

First a number of header files are included to get access to a few functions
and constants from the [C Standard library][stdlib].

[stdlib]: https://www.tutorialspoint.com/c_standard_library


### Global variable done

A global variable `done` is initialized to `false`.

``` C
bool done = false;
```

Later this variable is going to be updated by a signal handler.


### divide_by_zero

The `divide_by_zero` function attempts to [divide by zero][div-by-zero].

[div-by-zero]: https://en.wikipedia.org/wiki/Division_by_zero

### segfault

The function `segfault` attempts to  [dereference][dereference] a [NULL pointer][null-pointer]
causing a [segmentation fault][segfault].


[dereference]: https://en.wikipedia.org/wiki/Dereference_operator
[null-pointer]: https://en.wikipedia.org/wiki/Null_pointer
[segfault]: https://en.wikipedia.org/wiki/Segmentation_fault

### signal_handler

The `signal_handler` function will handle signals sent to
the process. A [switch statement][switch-statement] is used to determine which
signal has been received. An alternative is to use one signal handling function for each signal
but here a single signal handling function is used.

[switch-statement]: https://www.tutorialspoint.com/cprogramming/switch_statement_in_c.htm


[signals.h]: http://pubs.opengroup.org/onlinepubs/7908799/xsh/signal.h.html

### main

All C programs starts to execute in the `main` function.

- The process ID (PID) is obtained using the [getpid] function and printed to
  the terminal with [printf].
- A number of lines are commented out, we'll get back to these later.
- The function [puts] is used to print the string `I'm done!` on a separate line
  to the terminal.
- Finally, [exit] is used to terminate the process with exit status
  `EXIT_SUCCESS` defined in [stdlib.h].

[getpid]: http://pubs.opengroup.org/onlinepubs/009695399/functions/getpid.html
[printf]: https://www.tutorialspoint.com/c_standard_library/c_function_printf.htm
[puts]: https://www.tutorialspoint.com/c_standard_library/c_function_puts.htm
[exit]: https://www.tutorialspoint.com/c_standard_library/c_function_exit.htm
[stdlib.h]: https://www.tutorialspoint.com/c_standard_library/stdlib_h.htm

## Program, executable and process

Let's repeat the differences between a program, an executable and a process.

Program
: A set of instructions which is in human readable format. A
passive entity stored on secondary storage.

Executable
: A compiled form of a program including machine
instructions and static data that a computer can load and execute. A
passive entity stored on secondary storage.

Process
: An excutable loaded into memory and executing or waiting. A
process typically executes for only a short time before it either
finishes or needs to perform I/O (waiting). A process is an active
entity and needs resources such as CPU time, memory etc to execute.

## The make build tool

The [make] build tool is used together with the [Makefile] to compile all programs in
the `mandatory/src` directory.

[make]: https://en.wikipedia.org/wiki/Make_(software)
[Makefile]: https://en.wikipedia.org/wiki/Makefile

## Compile all programs

From a terminal, navigate to the `mandatory` directory. To compile all
programs, type `make` and press enter.

``` text
make
```

When compiling, `make` places all executables in the `bin` directory.

## First test run

Run the `signals` program.

``` text
./bin/signals
```

You should now see output similar to this in the terminal.

``` text
My PID = 81430
I'm done!
```

Note that the PID value you see will be different.

## New process

Run the program a few times. Note that each time you run the same program the
process used to execute the program gets a new process ID (PID).

## C comments

In C, `//` is used to start a comment reaching to the end of the line.

## Division by zero

To make the program divide by zero, uncomment the following line.

``` c
// divide_by_zero();
```

Compile with `make`.

``` text
make
```

Run the program.

``` text
./bin/signals
```

In the terminal you should see something similar to this.

``` text
My PID = 81836
[2]    81836 floating point exception  ./bin/signals
```


Division by zero causes an exception. When the OS handles the exception it sends
the `SIGFPE` (fatal arithmetic error) signal to the process executing the division by zero. The default
handler for the `SIGFPE` signal terminates the process and this is exactly
what happened here.


{{% notice style="warning" title="Division by zero on Mac" %}}

On some versions of Mac hardware, integer division by zero does not cause a
  `SIGFPE`  signal to be sent, instead a `SIGILL` (illegal instruction) signal
  is sent. On other combinations of Mac hardware and C compiler, some other
  signal might be sent, or no signal is sent at all.
  
  If you run on Mac hardware and the process does not terminate when dividing by
  zero, you can:
  
  -  try to catch the `SIGILL` signal instead of the
  `SIGFPE` signal
  - or, you could simply skip the whole division by zero part of
  this assignment. 

  Read more: 

  - [No run-time error for division by zero in c++ in
    mac?](https://www.sololearn.com/discuss/2410762/no-run-time-error-for-division-by-zero-in-c-in-mac)
  - [Integer division by zero but system shows execution with exit code 0](https://stackoverflow.com/questions/70930126/integer-division-by-zero-but-system-shows-execution-with-exit-code-0)
  - [trapping floating point exceptions and signal handling on Apple
    silicon](https://stackoverflow.com/questions/71821666/trapping-floating-point-exceptions-and-signal-handling-on-apple-silicon)
  - [On which platforms does integer divide by zero trigger a floating point exception?](https://stackoverflow.com/questions/37262572/on-which-platforms-does-integer-divide-by-zero-trigger-a-floating-point-exceptio)

{{% /notice %}}


Run the program a few times. Each time you run the program the same error
(division by zero) happens, causing an exception, causing the OS to send the
process the `SIGFPE` signal, causing the process to terminate.

{{% notice style="info" title="Synchronous signals" %}}

Synchronous signals are delivered to the same process that performed the operation that caused the
signal. Division by zero makes the OS send the process the synchronous signal
`SIGFPE`.

{{% /notice %}}

## Installing a signal handler

A program can install a signal handler using the `signal` function.

``` C
signal(sig, handler);
```

sig
: The signal you want to specify a signal handler for.

handler
: The function you want to use for handling the signal.

## Handling SIGFPE

Uncomment the following line to install the `signal_handler` function as the
signal handler for the `SIGFPE` signal.


``` C
// signal(SIGFPE,  signal_handler);
```

Compile with `make`.

``` text
make
```

Run the program.

``` text
./bin/signals
```

In the terminal you should see something similar to this.

``` text
My PID = 81979
Caught SIGFPE: arithmetic exception, such as divide by zero.
```

This time the signal doesn't terminate the process immediately. When the process
receives the `SIGFPE` signal the function `signal_handler` is executed with the
signal number as argument. After printing a message to the terminal the signal
handler terminates the process with status `EXIT_FAILURE`.

## No more division by zero

Comment out the following line.

``` c
divide_by_zero();
```

Compile and run the program Make sure you see output similar to this in the
terminal.

``` text
My PID = 82040
I'm done!
```

## Segfault

A segmentation fault (aka segfault) are caused
by a program trying to read or write an illegal memory location.
To make the program cause a segfault, uncomment the following line.

``` c
// segfault();
```

Compile with `make`.

``` text
make
```

Run the program.

``` text
./bin/signals
```

In the terminal you should see something similar to this.

``` text
My PID = 82084
[2]    82084 segmentation fault  ./bin/signals
```

The illegal memory access causes an exception. When the OS handles the exception it sends
the `SIGSEGV` signal to the process executing the illegal memory access. The default
handler for the `SIGSEGV` signal terminates the process and this is exactly
what happened here.

Run the program a few times. Each time you run the program the same error
(illegal memory access) happen, causing an exception, causing the OS to send the
process the `SIGSEGV` signal, causing the process to terminate.

{{% notice style="info" title="Synchronous signals" %}}

Synchronous signals are delivered to the same process that performed the operation that caused the
signal. An illegal memory access makes the OS send the process the synchronous signal
`SIGSEGV`.

{{% /notice %}}

## Handling SIGSEGV

Add code to install the function `signal_handler` as the signal handler for the
`SIGSEGV` signal.
When you run the program you should output similar to this in the terminal.

``` C
My PID = 82161
Caught SIGSEGV: segfault.
```

## No more segfault

Comment out the following line.

``` c
segfault();
```

Compile and run the program Make sure you see output similar to this in the
terminal.

``` text
My PID = 82040
I'm done!
```

## Wait for a signal

The `pause` function is used to block a process until it receives a signal (any
signal will do).
Uncomment the following line.

``` C
// pause();
```

Compile and run the program. You should see output similar to this in the
terminal.

``` text
My PID = 82249
```

The process is now blocked, waiting for any signal to be sent to the process.

## Ctrl+C

To terminate the process, press `Ctrl+C` in the terminal.

``` text
My PID = 82249
^C
```

Note that once the process terminates you get the terminal prompt back.


{{% notice style="info" title="Asynchronous signals" %}}

Asynchronous signals are generated by an event external to a running process.
Pressing `Ctrl+C` is an external event causing the OS to send the asynchronous `SIGINT`
(terminal interrupt) signal to the process.

{{% /notice %}}

The default signal `SIGINT` handler terminates the process.

## Handling SIGINT

Add code to install the function `signal_handler` as the signal handler for the
`SIGINT` signal.
When you run the program the process blocks waiting for any signal. When you
press `Ctrl+C` you should now see output similar to this in the terminal.

``` text 
My PID = 82477
^CCaught SIGINT: interactive attention signal, probably a ctrl+c.
I'm done!
```

## Open a second terminal

Open a second terminal.

## Sending signals from the terminal

Compile and run the program in one of the terminals. The program should block
waiting for any signal. Note the PID of the blocked process.

``` text
My PID = 82629
```

The command `kill` can be used to send signals to processes from the terminal.
To send the `SIGINT` signal to the blocked process, execute the following command in
the terminal where you replace `<PID>` with the PID of the blocked process.

``` text
kill -s INT <PID>
```

In the other terminal you should now see the blocked process execute the signal
handler, then continue in `main` after `pause()`, print `I'm done!` and terminate.

``` text
My PID = 82629
Caught SIGINT: interactive attention signal, probably a ctrl+c.
I'm done!
```

## Handle SIGUSR1

Add code to make the program print "Hello!" when receiving the `SIGUSR1` signal.

- Compile and run the program from one terminal.
- Send the `SIGUSR1` signal to the process from the other terminal using the
`kill` command where you replace `<PID>` with the PID of the blocked process.

``` text
kill -s SIGUSR1 <PID>
```

## Don't terminate on SIGUSR1

How can you make the program print `Hello!` every time the signal `SIGUSR1` is
received without terminating?

### Set the global variable done to true

In the `signal_hanlder` function, set the global variable `done` to `true` when handling the
`SIGINT` signal.

### Block until done

In `main`, replace the line:

``` C
pause();
```

, with the following while loop:

``` C
while (!done);
```

In C `!` is the logical not operator. This `while` loop repeatedly checks the global
variable `done` until it becomes `true`.

### Compile, run and test

Compile and run the program from one terminal and send signals to the process from
the other terminal.

- Are you able  to send multiple `SIGUSR1` signals to the process?
- Are you able to break out of the `while` loop and terminate the process by sending the signal `SIGINT` to the
process, or by pressing `Ctrl+C` from the terminal?

### Bug?

Depending on your compiler the program may not break out of the `while(!done)` loop

{{% notice style="info" title="An optimizing compiler" %}}

An optimizing compiler may detect that the variable `done` is not changed in the
`while(!done);` loop and replace the loop with `if (!false);`.

{{% /notice %}}

### Volatile

Do you remember the volatile keyword?

{{% notice title="Volatile" %}}

The `volatile` keyword is used to make sure that the contents of a variable
is always read from memory.

{{% /notice %}}

Make the global variable `done` volatile.

### Compile, run and test

Compile and run the program from one terminal and send signals to the process from
the other terminal.

- Make sure you are able to send multiple `SIGUSR1` signals to the process.
- Make sure you can terminate the process by sending the signal `SIGINT` to the
process, or by pressing `Ctrl+C` from the terminal.

### sig_atomic_t

The data type `sig_atomic_t` guarantees that reading and writing a variable happen in a single
instruction, so there’s no way for a signal handler to run “in the middle” of an
access. In general, you should always make any global variables changed by a
signal handler be of the data type `sig_atomic_t`.

Change the datatype of the global variable `done` from `bool` to `sig_atomic_t`.


### Use pause instead

Using a `while` loop to repeatedly check the global variable `done` is not a
very efficient use of the CPU. A better way is to change the loop to:

``` C
while (pause()) {
  if (done) break;
};
```

Why is this more efficient?


## Code grading questions

Here are a few examples of questions that you should be able to answer, discuss
and relate to the source code of you solution during the code grading.

- What is a signal?
- How do signals relate to exceptions and interrupts?
- What is a signal handler?
- How do you register a signal handler?
- What happens if you don’t register a signal handler?
- What causes a segfault?
- What is meant by a synchronous signal?
- What do the systemcall pause() do?
- What happens when you press Ctrl-C in the controlling terminal of a process?
- How do you send signals to other processes?
- Why is the keyword volatile needed when declaring the global variable done?
- Why is the datatype sig_atomic_t needed when declaring the global variable done?
- Why is it more efficient to use pause() instead of simply loop and check the done variable?

