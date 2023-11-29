---
title: Clone repository
assignment: github
weight: 20
---


Before you continue, you must clone the [mips-examples][repo] repository.

[repo]: https://github.com/os-ospp-dsp/mips-examples.git


## Use the git command

From the terminal, navigate to a directory where you want the cloned directory
to be created and execute the following command.

``` text
$ git clone https://github.com/os-ospp-dsp/mips-examples.git
```

Now you should see something similar to this written to the terminal.

``` text
Cloning into 'mips-examples'...
remote: Counting objects: 9, done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 9 (delta 0), reused 9 (delta 0), pack-reused 0
Unpacking objects: 100% (9/9), done.
Checking connectivity... done.
```

## Use the tree command

To get an overview of the cloned repository, use the `tree` command.

``` text
$ tree mips-examples
```

Now you should see a tree view of all files and directories in the
`mips-examples` directory.

``` text
mips-examples/
├── README.md
├── arrays.s
├── basics.s
├── hello.s
└── jump_and_branches.s

0 directories, 5 files
```

You are now ready to continue with the [Introduction to MARS](introduction-to-mars) tutorial.

