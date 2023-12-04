------
title: The all utility
weight: 52
draft: false
---

In the `threads-synchronization-deadlock` directory you find the `all` utility script.

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

