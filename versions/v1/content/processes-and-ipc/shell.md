---
title: Shell
assignment: higher-grade
weight: 50
draft: false
---

<h2 class="subtitle">Optional assignment for higher grade</h2>

A shell is an interface between a user and the
operating system. It lets us give commands to the system and start other
programs.
Your task is to program a simple [shell][wp-shell] similar to for
example [Bash][wp-bash], which probably is the command shell you normally use
when you use a Unix/Linux system. 

[wp-shell]: https://en.wikipedia.org/wiki/Shell_(computing)
[wp-bash]: https://en.wikipedia.org/wiki/Bash_(Unix_shell)

## Preparations

When programming a shell, several of the POSIX system calls you studied already
will be useful. Before you continue, make sure you have a basic understanding of
at least the following system calls: `fork()`, `execvp()`, `getpid()`, `getppid()`,
`wait()`, `pipe()`, `dup2()` and `close()`.

## Grade 4

For grade 4 your shell must be able to handle a single command and a pipeline with two commands.

## Grade 5

For grade 5 your shell must be able to handle a single command or a pipeline
with two, three or more commands. 

You must also make sure not to leak resources.
Especially you must make sure, that after a command line has finished, all
descriptor to created pipes are closed. Otherwise the operating system will not
be able to deallocate the pipes and potentially re-use the descriptor values. 

## Command data

In the file `module-2/higher-grade/src/parser.h` the following  C structure is defined. 

``` C
/**
 * Structure holding all data for a single command in a command pipeline.
 */
typedef struct {
  char*  argv[MAX_ARGV];  // Argument vector.
  position_t pos;         // Position within the pipeline (single, first, middle or last).
  int in;                 // Input file descriptor.
  int out;                // Output file descriptor.
} cmd_t;
```

The above structure is used to hold data for a single command in a command pipe
line. 

## Command array

In the file `module-2/higher-grade/src/shell.c` command data for all commands in
a command pipeline is stored in a global array.

``` C
/**
 * For simplicitiy we use a global array to store data of each command in a
 * command pipeline .
 */
cmd_t commands[MAX_COMMANDS];
```

## Parser

In `module-2/higher-grade/src/parser.h ` you find the following prototype. 

``` C
/**
 *  parses the string str and populates the commands array with data for each
 *  command.
 */
int parse_commands(char *str, cmd_t* commands);
```

This function [parse][wp-parsing] a command line string `str` such as `"ls -l -F
| nl" ` and populates  the `commands` array with command data.


[wp-parsing]: https://en.wikipedia.org/wiki/Parsing#Computer_languages

## Program design

The below figure shows the overall structure of the shell. 

![](/v1/images/module-2/shell-pipeline.png)

When a user types a command line on the form `A | B | C` the parent parses the
user input and creates one child process for each of the commands in the
pipeline. The child processes communicates using pipes. Child A redirects stdout
to the write end of pipe 1. Child B redirects stdin to the read end of pipe 1
and stdout to the write end of pipe 2. Child C redirects stdin to the read end
of pipe 2.

## parser.c

In the `module-2/higher-grade/src/parser.c` file you must complete the
implementation of the following function. 

``` C
/**
 * n - number of commands.
 * i - index of a command (the first command has index 0).
 *
 * Returns the position (single, first, middle or last) of the command at index
 * i.
 */
position_t cmd_position(int i, int n) {
```

## shell.c

Use  `module-2/higher-grade/src/shell.c` to implement your solution. This file
already implements the most basic functionality but it is far from complete.

## Feel free to make changes 

When inplementing your solution, you are allowed to:

- add your own functions
- modify existing functions
- add (or remove) arguments to existing functions
- modify existing data structures
- add you own data strutures.

## Alternative program design

The provided code is based on a design where the shell (parent) process creates
one child process for each command in the pipeline. 

An alterantive design is for the first child process to create the second child,
the second child to create the third child etc. If you prefer this design, this
might require you to modify the provided source code to fit this alterantive
design. 