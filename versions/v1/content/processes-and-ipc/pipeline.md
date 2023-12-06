---
title: Pipeline
assignment: mandatory
weight: 40
draft: false
---

<h2 class="subtitle">Mandatory assignment</h2>

Your task is to create a system with one parent process and two
child processes where the children communicate using a pipe.

![](/v1/images/processes-and-ipc/parent-children-pipe.png?width=600px)

## Open a terminal

Open a terminal and navigate to the `mandatory` directory.

## List directory contents (ls)

The `ls` shell command list directory content. Execute the `ls` command in the
terminal.

``` text
ls
```

You should now see this: 

``` text
Makefile     bin     obj     src
```


The `-F` option appends a slash `/` to directory entries.

``` text
ls -F
```

The only file in the directory is `Makefile`. The directory contains the
subdirectories `bin`, `obj` and `src`.

``` text
Makefile     bin/     obj/     src/
```

Add the `-1` option to:

``` text
$ ls -F -1
```

, print each entry on a separate line:

``` text
Makefile
bin/
obj/
src/
```

## Line numbering filter (nl)

The `nl` shell command is a filter that reads lines from stdin and echoes each
line prefixed with a line number to stdout. In the terminal, type `nl` and press
enter.

``` text
nl
```

The `nl` command now waits for stdin input. Type `Some text` and press enter.

``` text
Some text
     1 Some text
```

Type `More text` and press enter.

``` text
More text
     2 More text
```

For each line you type, the line is echoed back with a line number. Play around
some more with the `nl` command. Press `Ctrl+D` (End Of File) to make the `nl`
command exit.

## Pipeline

One of the philosophies behind Unix is the
motto [do one thing and do it well][wp-unix-dotadiw]. In this spirit, each shell command
usually have a very specific purpose. More complicated commands can be
constructed by combining simpler commands in a [pipeline][wp-pipeline] such that the output
of one command becomes the input to another command.

The standard shell syntax for
pipelines is to list multiple commands, separated by vertical bars `|` (the pipe
character). In the below example the output from the `ls -F -1`
command is piped to input of the  `nl` command.

[wp-unix-dotadiw]: https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well
[wp-pipeline]: https://en.wikipedia.org/wiki/Pipeline_(Unix)

``` text
ls -F -1 | nl
```

Now the result of `ls -F -1` is printed with line numbers added at the front of each line. 

``` text
    1 Makefile
    2 bin/
    3 obj/
    4 src/
```

## Shell commands are ordinary programs

All shell commands are ordinary programs. When the shell executes a command, the
shell first forks a new process and uses exec to make the new process execute
the command program.

## Which programs

The `which` utility locates a program executable in the user's [PATH][wp-PATH]. Use `which` to
see which program executables  the shell uses for the `ls` and `nl` commands.

[wp-PATH]: https://en.wikipedia.org/wiki/PATH_(variable)

``` text
which ls
```

The executables for the `ls` command program is found in the `/bin` directory.

``` text
/bin/ls
```

What about the `nl` command?

``` text
$ which nl
```

The executables for the `nl` command programs is also  located in the
`/bin` directory.

``` text
/bin/nl
```

## System overview

Your task is to create a program that mimics the `ls -F -1 | nl` pipeline. The
parent process uses fork to create two child processes. Child A uses exec
to execute the `ls -F -1` command and Child B uses exec to execute the
`nl` command. The children uses a pipe to communicate. The stdout of Child A is
redirected to the write end of the pipe. The stdin of the Child B is redirected
to the read end of the pipe.

![](/v1/images/processes-and-ipc/parent-ls-nl.png)


## Pipe

How can you make the child process share a pipe? Which process should create the
pipe? When should the pipe be created?

## Parent forks twice

The parent must use fork twice, once for each child process.

## Parent waits twice

The parent should use `wait` to twice to wait for both child process to
terminate.

## The children uses execlp

The child process should use [execlp](exec#execlp) to execute their
commands.

## The children uses dup2

The child process should use `dup2` to redirect stdout and stdin.

- Process A redirects stdout to write to the pipe.
- Process B redirects stdin to read from the pipe.

## Close unused pipe file descriptors

Don't forget to close unused pipe file descriptors, otherwise a reader or writer
might be blocked .

| Attempt | Conditions                     | Result                     |
| ------- | ------------------------------ | -------------------------- |
| Read    | Empty pipe, writers attached   | Reader blocked             |
| Read    | Empty pipe, no writer attached | End Of File (EOF) returned |
| Write   | Full pipe, readers attached    | Writer blocked             |
| Write   | No readers attached            | SIGPIPE                    |

Closing a read descriptor to early may cause a SIGPIPE.

{{% notice style="warning" title="Beware of SIGPIPE" %}}
The default SIGPIPE signal handler causes the process to terminate.
{{% /notice %}}

## Check return values

You should check the return values of all system calls to detect errors. On error use
[perror] to print an error message and then terminate with `exit(EXIT_FAILURE)`.

[perror]: https://www.tutorialspoint.com/c_standard_library/c_function_perror.htm

## pipeline.c

Use the file `src/pipeline.c` to implement your solution.

- You must add code the the `main` function. You must add code here.
- After fork, Child A should execute the `child_a` function. You must add code here.
- After fork, Child B should execute the `child_b` function. You must add code here.
- You may add your own functions to further structure your solution.

## Compile and run

Use make to compile.

``` text
$ make
```

Run the program.

``` text
$ ./bin/pipeline
```

When running the finished program you should see output similar to this.

``` text
$ ./bin/pipeline
    1 Makefile
    2 bin/
    3 obj/
    4 src/
$
```

Make sure you get the shell prompt `$` back.

## Code grading questions

Here are a few examples of general questions not directly related to the source
code that you should be able to answer and discuss during the code
grading.

- What do we mean with a process pipeline?
- What is a pipe?
- How are pipes used to construct process pipelines?
- Show an example of a proccess pipeline in the terminal (shell).
- How are shell (terminal) commands implemented?
- What happens to the file descriptor table after a successful creation of a new pipe?

Here are a few examples of questions that you should be able to answer, discuss
and relate to the source code of you solution during the code grading.

- How many times does the parent calls fork and why?
- Why do the children need to call execlp()?
- Explain how each child is able to redirect stdin or stdout from or to the pipe?
- How will the consumer know when there is no more data to expect from the pipe?
- Why is it important for a process to close any pipe file descriptors it does not intend to use?
- What could happen if you close a read descriptor to early?
