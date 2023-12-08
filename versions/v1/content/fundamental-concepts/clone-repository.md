---
title: Clone repository
weight: 60
assignment: github
draft: false
---

Before you continue, you must clone the [fundamental-os-concepts][repo] repository.

[repo]: https://github.com/os-assignments/fundamental-os-concepts.git


## Use the git command

From the terminal, navigate to a directory where you want the cloned directory
to be created and execute the following command.

``` bash session
git clone https://github.com/os-assignments/fundamental-os-concepts.git
```

Now you should see something similar to this in the terminal.

``` bash session
Cloning into 'fundamental-os-concepts1'...
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 7 (delta 0), reused 7 (delta 0), pack-reused 0
Unpacking objects: 100% (7/7), done.
```

## Use the tree command

To get an overview of the cloned repository, use the `tree` command.

``` bash session
tree fundamental-os-concepts
```

Now you should see a tree view of all files and directories in the
`fundamental-os-concepts` directory.

``` bash session
fundamental-os-concepts
├── README.md
├── higher-grade
│   └── multiprogramming.s
└── mandatory
    └── exceptions-and-interrupts.s

2 directories, 3 files
```

{{% notice style="warning" title="Install tree on macOS" %}}
If you run **macOS** and `tree` is not installed, use [Homebrew](https://brew.sh/) to install `tree`.

``` bash session
brew install tree
```
{{% /notice %}}