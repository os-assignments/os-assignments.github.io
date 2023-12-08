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
- [Mips assembly][mips] will be used to study the fundamental principles of how
  the operating system interacts with the hardware. To edit and execute Mips assembly programs we will use [Mars][mars] (Mips
Assembler and Runtime Simulator). 
- [C programming][c] in an [Linux or macOS](supported-systems) environment will be used to further study a few important concepts.
- [Make][make] will be used to compile all C programming assignments. 


[mars]: http://courses.missouristate.edu/kenvollmar/mars/

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

## Toolbar

At the top of each page you find the toolbar with the following buttons.

<table class="icon-list">
<tr>
  <td>
    {{% icon bars %}}
  </td>
  <td>
    This button will only appear if the main menu to the left is hidden. Press this button to show
    the main menu. 
  </td>
</tr>
<tr>
  <td>
    {{% icon list-alt %}}
  </td>
  <td>
    View the table of contents menu for the page.
  </td>
</tr>
<tr>
  <td>
    {{% icon print %}}
  </td>
  <td>
    Show printer friendly version of the current page and all its subpages. 
  </td>
</tr>
<tr>
  <td>
    {{% icon pen %}}
  </td>
  <td>
    Suggest edits of the page by making a pull request on GitHub. 
  </td>
</tr>
<tr>
  <td>
    {{% icon chevron-left %}}
  </td>
  <td>
    Navigate to the next page in the hierarchy.
  </td>
</tr>
<tr>
  <td>
   {{% icon chevron-right %}}
  </td>
  <td>
    Navigate to the previous page in the hierarchy.
  </td>
</tr>
</table>


## Main menu

To the left of each page you find the main menu. If the main menu is hidden, press the {{%
icon bars %}} icon in the toolbar at the top of the page.  The following
symbols are used in the main menu.  


<table class="icon-list">
<tr>
  <td>
   {{% assignmentIcon github %}}
  </td>
  <td>
    Instructions on how to download source code from GitHub.
  </td>
</tr>
<tr>
  <td>
    {{% assignmentIcon mandatory %}}
  </td>
  <td>
   Mandatory assignment. 
  </td>
</tr>
<tr>
  <td>
   {{% assignmentIcon higher-grade %}}
  </td>
  <td>
     Optional assignment for higher grade.
  </td>
</tr>
</table>

At the bottom of the main menu you find these two buttons. 
<table class="icon-list">
<tr>
  <td>
 {{% icon icon="tint" %}}
  </td>
  <td>
    Switch between viewing the website in dark mode or light mode.
  </td>
</tr>
<tr>
  <td>
   {{% icon icon="history" %}}
  </td>
  <td>
  Clear the {{% icon check %}} markings of the pages
  you have visited. 
  </td>
</tr>
</table>

## Shell commands and code snippets 

Shell commands will be shown in boxes like this. 

``` text
make # Run make to compile
```

Code snippets will be shown in boxes like this. 

``` C
int a = 127;    // A global variable. 

int main(void) {
  int b = 42;   // A local variable.
}
```

If you hover over boxes like these, the {{% icon icon="copy" %}} icon will appear in the upper
right corner of the box. Press this icon to copy the context of the box. 

## Author

This webpage and all assignments have been created by [Karl Marklund][km] at the
[Department of information technology][it], [Uppsala university][uu]. The
original version of the [Mutual exclusion][mutex] assignment was contributed by [Nikos
Nikoleris][nn] when he worked as TA on the courses during his time as a PhD student.

[km]: https://www.katalog.uu.se/profile/?id=N2-482
[nn]: https://www.arm.ecs.soton.ac.uk/people/dr-nikos-nikoleris/
[mutex]: threads-and-synchronization/mutex/