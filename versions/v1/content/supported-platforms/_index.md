---
title: Supported platforms
weight: -10
---

![](/v1/images/supported-systems/linux-macOS.png?width=400px)

[Linux][linux] and [macOS][macOS] are the only two fully supported platforms for the programming
assignments. 

[linux]: https://en.wikipedia.org/wiki/Linux

## Linux and macOS

The programming assignments have been developed and tested on the [department
Linux system][dep-linux] and [macOS][macOS]. 


## Mips assembly

To edit and execute Mips assembly programs we will use [Mars][mars] (Mips
Assembler and Runtime Simulator). 

- Mars is available on the [department Linux system][dep-linux]. 
- Mars will run on any system (including Windows) as long as you have [Java
installed][java-install]. If you prefer, you may [download][download] and
install Mars on your private computer.

[mips]: https://en.wikipedia.org/wiki/MIPS_instruction_set

[mars]: http://courses.missouristate.edu/kenvollmar/mars/

[java]: https://en.wikipedia.org/wiki/Java_(software_platform)

[java-install]: https://java.com/en/download/help/index_installing.xml

[download]: http://courses.missouristate.edu/KenVollmar/mars/download.htm

## C programming

The C programming assignments have been developed and tested on the [department
Linux system][dep-linux] and [macOS][macOS]. 

- In practice this means that you should most likely be able to do the C programming
assignments on any computer running Linux or macOS.

## Remote login to the department Linux system with SSH

You can access the [department Linux System ][dep-linux] remotely using 
[SSH][ssh-wp] to login to one of the [student servers][linux-hosts].

- You will not be able to access the graphical desktop environment.
- But, you can start graphical applications from the command line (shell) if you use [X
forwarding][x-forwarding] together with SSH. 

In the below example, a user with user name `abcd1234` uses the `ssh` command
with the `-X` option to enable X forwarding to log in to the department Linux
server `trygger.it.uu.se`.

[x-forwarding]: https://en.wikipedia.org/wiki/X_Window_System#Remote_desktop

[linux-hosts]: http://www.it.uu.se/datordrift/maskinpark/linux

[ssh-wp]: https://en.wikipedia.org/wiki/SSH_(Secure_Shell)

{{% tab title="Local shell (your computer)" %}}
``` bash session
ssh -X abcd1234@trygger.it.uu.se
```
{{% /tab %}}

One you are logged in, you can start graphical applications from the Linux
shell. You can for example run [Mars](prerequisites/mips-and-mars/). 

{{% tab title="Remote shell (department Linux system)" %}}
``` bash session
mars
```
{{% /tab %}}

![](/v1/images/mars/MARS_hello.png)

To edit C source code you can for example use the [VS Code][vscode] source code
editor remotely. 

[vscode]: https://en.wikipedia.org/wiki/Visual_Studio_Code

{{% tab title="Remote shell (department Department Linux system)" %}}
``` bash session
code
```
{{% /tab %}}

![](/v1/images/prerequisites/linux/vscode.png)


## Linux inside Windows

If you are using Windows and don't like working remotely with the Deparment
Linux system, nor do you want to install Linux alongside Windows on
your computer (dual boot),consider one of the following options: 

- Install and use the [Windows subsystem for Linux][wsl].
- Install [VirtualBox][virtualbox] and run a virtual Linux machine. This [tutorial][installing-ubuntu-on-virtual-box] will cover how to install
VirtualBox and set up your first virtual machine, show you how to get Ubuntu and
prepare for installation, and walk you through an installation of Ubuntu.

[macOS]: https://en.wikipedia.org/wiki/MacOS
[dep-linux]: prerequisites/department-linux-system/
[wsl]: https://learn.microsoft.com/en-us/windows/wsl/about

[virtualbox]: https://en.wikipedia.org/wiki/VirtualBox
[installing-ubuntu-on-virtual-box]: http://www.wikihow.com/Install-Ubuntu-on-VirtualBox

