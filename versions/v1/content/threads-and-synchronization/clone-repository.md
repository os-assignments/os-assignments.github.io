------
title: Clone repository
weight: 50
assignment: github
draft: false
---

Before you continue, you must clone the [threads-synchronization-deadlock][repo]
repository.

[repo]: https://github.com/os-assignments/threads-synchronization-deadlock.git

## Use the git command

From the terminal, navigate to a directory where you want the cloned directory
to be created and execute the following command.

``` text
git clone https://github.com/os-assignments/threads-synchronization-deadlock.git
```

Now you should see something similar to this in the terminal.

``` text
Cloning into 'threads-synchronization-deadlock'...
remote: Counting objects: 23, done.
remote: Compressing objects: 100% (20/20), done.
remote: Total 23 (delta 1), reused 23 (delta 1), pack-reused 0
Unpacking objects: 100% (23/23), done.
```

## Use the tree command

To get an overview of the cloned repository, use the `tree -d` command.

``` text
tree -d threads-synchronization-deadlock
```

Now you should see a tree view of the directory strucure.

``` text
threads-synchronization-deadlock
├── examples
│   ├── bin
│   ├── obj
│   └── src
├── higher-grade
│   ├── bin
│   ├── obj
│   └── src
└── mandatory
    ├── bin
    ├── obj
    ├── psem
    └── src

13 directories
```

## The all utility

The `all` utility can be used to run a shell command in all subdirectories,
i.e., run a command in all of the three directories `examples`, `higher grade`
and `mandatory`. In the terminal, navigate to the `threads-synchronization-deadlock` directory.

For example, you can use the `all` utility together with `make` to compile all
programs.

``` C
./all make
```

The `all` utility can also be used to delete all objects files and executables.

``` C
./all make clean
```

