---
title: Operating systems assignments
archetype: home
---

![](images/uu-full-logo-dark.png?classes=uu-full-logo)

Operating systems programming assignments for the OS (1DT044), OSPP (1DT096) and
DSP (1DT003) courses at the [Department of information technology][it], [Uppsala
university][uu].

[it]: https://www.it.uu.se/first?lang=en

[uu]: https://www.uu.se/en/

- Our studies will be based on principles of the [Unix](#unix) and
  [Linux](#linux) operating systems.
- For all assignments, you will [use Git to clone provided source code from repositories on GitHub][git/github]. 
- [Mips assembly][mips] will be used to study the fundamental principles of how the operating system interacts with the hardware.
- [C programming][c] in an Linux or macOS environment will be used to further study a few important concepts.
- [Make][make] will be used to compile all C programming assignments. 

[make]: https://en.wikipedia.org/wiki/Make_(software)

[unix/linux]: unix-and-linux

[mips]:  prerequisites/mips-and-mars

[c]:  prerequisites/c

[linux]: prerequisites/linux

[git/github]: prerequisites/git-and-github/

## Unix

[Unix](https://en.wikipedia.org/wiki/Unix) is a family of [multitasking][multitasking],
multiuser computer operating systems that derive from the original AT&T Unix,
developed in the 1970s at the Bell Labs research center by Ken Thompson, Dennis
Ritchie, and others[^unix].

[^unix]: [https://en.wikipedia.org/wiki/Unix](https://en.wikipedia.org/wiki/Unix)

[multitasking]: https://en.wikipedia.org/wiki/Computer_multitasking

## Linux

[Linux](https://en.wikipedia.org/wiki/Linux) is a name which broadly denotes a
family of free and [open-source](https://en.wikipedia.org/wiki/Open-source_software) software operating system distributions built
around the [Linux kernel](https://en.wikipedia.org/wiki/Linux_kernel). The defining component of a Linux distribution is the
Linux kernel, an operating system kernel first released on September 17,
1991 by [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds)[^linux].

Linux is a [Unix-like](https://en.wikipedia.org/wiki/Unix-like) and
mostly [POSIX-compliant](https://en.wikipedia.org/wiki/POSIX) computer operating
system[^linux]. As of May 2015 it is estimated that 96.55% of all web servers
runs Linux [^linux-market-share]. 

[^linux]: [https://en.wikipedia.org/wiki/Linux](https://en.wikipedia.org/wiki/Linux)

[^linux-market-share]:
    [https://en.wikipedia.org/wiki/Linux#Market_share_and_uptake](https://en.wikipedia.org/wiki/Linux#Market_share_and_uptake)
    
## The basic principles of Unix and Linux

Unix has a very long history both in industry and academia. Compared to
proprietary operating systems such as [Microsoft
Windows](https://en.wikipedia.org/wiki/Microsoft_Windows) and
[macOSX](https://en.wikipedia.org/wiki/MacOS) detailed information about the
design and implementation of Unix and Linux is much easier to find. As a
consequence, Unix/Linux is well suited when studying and learning the core
principles of operating systems. 

In the the operating systems courses at Uppsala University both Unix and Linux
will be used to introduce the basic principles of operating systems. 

## Supported platforms for programming assignments

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

The C programming assignments have been developed and tested on [macOS][macOS] and the
[department Linux system][dep-linux]. 

- In practice this means that you should be able to do the C programming
assignments on any computer running macOS or Linux. 


If you are using Windows you could try to install out the [Windows subsystem for Linux][wsl]. 
If you don't want to install Linux on your computer, you can try to install
[VirtualBox][virtualbox] and run a virtual Linux machine. 
This [tutorial][installing-ubuntu-on-virtual-box] will cover how to install VirtualBox and set up your first virtual
machine, show you how to get Ubuntu and prepare for installation, and walk you
through an installation of Ubuntu.

[macOS]: https://en.wikipedia.org/wiki/MacOS
[dep-linux]: prerequisites/linux/department-linux-system/
[wsl]: https://learn.microsoft.com/en-us/windows/wsl/about

[virtualbox]: https://en.wikipedia.org/wiki/VirtualBox
[installing-ubuntu-on-virtual-box]: http://www.wikihow.com/Install-Ubuntu-on-VirtualBox



## Menu

To the left of each page you find the menu. If the menu is hidden, press the {{%
icon bars %}} icon on the top of the page to view/hide the menu. The following
symbols are used in the menu.  

- Instructions on how to download source code from GitHub are marked with {{%
  assignmentIcon github %}}.
- Mandatory assignments are marked with {{% assignmentIcon mandatory %}}. 
- Optional assignments for higher grade are marked with {{% assignmentIcon
  higher-grade %}}.

At the bottom of the menu you see two icons. 

- Use {{% icon icon="tint" %}} to select between viewing in dark or light mode. 
- Use {{% icon history %}} to clear the {{% icon check %}} markings of the pages
  you have visited. 