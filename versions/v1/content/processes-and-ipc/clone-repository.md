---
title: Clone repository
weight: 5
assignment: github
draft: false
---

Before you continue, you must clone the [processes-and-ipc][repo] repository.

[repo]: https://github.com/os-assignments/processes-and-ipc.git

## Use the git command

From the terminal, navigate to a directory where you want the cloned directory
to be created and execute the following command.

``` text
git clone https://github.com/os-assignments/processes-and-ipc.git
```

Now you should see something similar to this in the terminal.

``` text
Cloning into 'processes-and-ipc'...
remote: Counting objects: 38, done.
remote: Compressing objects: 100% (26/26), done.
remote: Total 38 (delta 8), reused 38 (delta 8), pack-reused 0
Unpacking objects: 100% (38/38), done.
Checking connectivity... done.
Checking out files: 100% (32/32), done.
```

## Use the tree command

To get an overview of the cloned repository, use the `tree` command.

``` text
tree processes-and-ipc
```

Now you should see a tree view of all files and directories in the
`processes-and-ipc` directory.

``` text
processes-and-ipc
├── examples
│   ├── bin
│   ├── data
│   │   └── data.txt
│   ├── Makefile
│   ├── obj
│   └── src
│       ├── child.c
│       ├── execlp_ls.c
│       ├── execv_ls.c
│       ├── execvp_ls.c
│       ├── fork.c
│       ├── fork_exec.c
│       ├── fork_exit_wait.c
│       ├── fork_exit_wait_status.c
│       ├── fork-template.c
│       ├── fork_zombie.c
│       ├── ls_pipe_wc.c
│       ├── open_read.c
│       ├── perror.c
│       ├── pipe.c
│       └── random_mystery.c
├── higher-grade
│   ├── bin
│   ├── Makefile
│   ├── obj
│   └── src
│       ├── parser.c
│       ├── parser.h
│       └── shell.c
├── mandatory
│   ├── bin
│   ├── Makefile
│   ├── obj
│   └── src
│       ├── pipeline.c
│       └── signals.c
└── tools
    ├── monitor
    └── my-ps

14 directories, 26 files
```

{{% notice style="warning" title="Install tree on macOS" %}}
If you run **macOS** and `tree` is not installed, use [Homebrew](https://brew.sh/) to install `tree`.

``` text
brew install tree
```
{{% /notice %}}
