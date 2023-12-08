---
title: Git and GitHub
weight: 40
---

![](/v1/images/prerequisites/git-and-github/git-github.jpg?width=400px)

Source code for all tutorials and assignments will be made available in
various [repositories][repos] on [GitHub][github]. To
download source code you will use [Git][git] to clone repositories to your
Department Linux user account or to your private computer.

[repos]: https://github.com/os-ospp-dsp
[git]: https://en.wikipedia.org/wiki/Git
[github]: https://en.wikipedia.org/wiki/GitHub
[repository]: https://en.wikipedia.org/wiki/Repository_(version_control)

## Check if you already have Git installed

Open a terminal, type `git --version` and press enter. 

``` bash session
git --version
```

If `git` is installed you will something similar to this as a result in the
terminal.

``` bash session
git version 1.9.1
```

If `git` is not installed you will something similar to this as a result in the
terminal.

``` bash session
git: command not found
```

## Install Git

If `git` is not installed on your system, install `git` by following
these
[instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## Example GitHub repository

Let's look at the following [example][example-repo] GitHub repository.

[example-repo]: https://github.com/os-ospp-dsp/git-example-repo

![](/v1/images/prerequisites/git-and-github/example-github-repository.png?classes=border)

The repository contains the folder `sub` and the files `README.md`, `one.txt`
and `two.txt`. The `sub` folder contains a single file named `third.txt`.
By clicking on a file, for example the `one.txt` file,  you will see the content of the file.

``` bash session
A small text file.
```

## Cloning a repository

Click on the green button named **Code** (1). Now a small pop-up will
appear showing the repository URL. To copy the repository URL click on
copy-to-clipboard icon <i class="far fa-clone"></i> (2) to the right of the URL.

![](/v1/images/prerequisites/git-and-github/example-github-repository-clone-or-download-copy-to-clipboard.png)

From the terminal, navigate to a directory where you want the cloned directory
to be created. To clone the example repository, type `git clone` followed by a space and paste the repository URL. 

``` bash session
$ git clone https://github.com/os-assignments/git-example-repo.git
```

Press **enter** to execute the command. Now you should see something similar to this written to the terminal.

``` bash session
Cloning into 'git-example-repo'...
remote: Counting objects: 14, done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 14 (delta 1), reused 14 (delta 1), pack-reused 0
Unpacking objects: 100% (14/14), done.
Checking connectivity... done.
```

Note the first line `Cloning into 'git-example-repo'...`. This tell
you that the cloned repository is found in the newly created directory `git-example-repo`
within the current working directory.

## Investigate the cloned directory

To get an overview of the cloned repository, use the `tree` command.

``` bash session
tree git-example-repo
```

The `tree` command will print out a tree view showing all files and folders in
the `git-example-repo` directory.

``` bash session
git-example-repo
├── README.md
├── one.txt
├── sub
│   └── three.txt
└── two.txt

1 directory, 4 files
```

From the above we see that in the `git-example-repo` we find the files
`README.md`, `one.txt` and `two.txt`. In the same directory we also find a sub
directory named `sub`. Within the `sub` directory we find a single file named
`three.txt`.
