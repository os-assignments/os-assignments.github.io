---
title: The department Linux system
weight: 20
---

Here you find information on how to log in to the University's Linux system
from one of the computer rooms for students on [Campus Ångströmlaboratoriet][campus].

[campus]: https://www.uu.se/en/campus/angstrom-laboratory
## Windows in all computer rooms

In all computer rooms on campus Ångstrumslaboratoriet you find computers with
Windows. 

![Computer screen, keyboard and mouse](/v1/images/prerequisites/linux/hus-10-pc.jpg?width=400px)

## ThinLinc

On campus Ångströmlaboratoiet there are a number of large computers ([servers][server]) running
Linux somewhere in a basement. As a student, you will never physically interact
with these server computers.  Instead, you log in to these servers from Windows
to get access to the Linux desktop environment on the same screen as Windows. The system used
for this is called [ThinLinc][thinlinc].

[server]: https://en.wikipedia.org/wiki/Server_(computing)

[thinlinc]: https://en.wikipedia.org/wiki/ThinLinc

{{% notice style="info" title="Access Linux from Windows" %}}

To access the Linux system, you must first log into Windows and from
Windows use ThinLinc Client to log in to a Linux server.

{{% /notice %}}

## Log in to Windows

To log in to Windows, enter the username of your **student account** on
the form `abcd1234` and **Password A**.

![Windows login](/v1/images/prerequisites/linux/windows-10-login.jpg?width=400px)

## Software Center (ZENworks)

Somewhere on the Windows desktop you will find **Software center (Zenworks)**.

![Software Center (Zenworks) icon on the Windows desktop](/v1/images/prerequisites/linux/software-center-icon.png?width=400px)

Double-click the **Software center (Zenworks)** icon. Now a new windows opens with
available software.

![Software center (Zenworks)](/v1/images/prerequisites/linux/software-center.png?width=400px)

The software you find here may already be installed on the computer.
If a program is not already installed, it will be installed the first time you
trying to use the program.

## Start the ThinLinc client


From **Software Center**, find the  **ThinLinc client**.

![ThinLinc Client icon](/v1/images/prerequisites/linux/software-center-thinlinc-client-icon.png?width=600px)

Double-click on **ThinLinc Client**. If ThinLinc Client is already installed
, it will start quite quickly. If not installed
already, ThinLInc Client will first be installed and then started. 

## The Windows taskbar

Once the ThinLinc client started, the ThinLinc client icon will appear in the
Windows **taskbar**.

![ThinLinc icon in the Windows taskbar](/v1/images/prerequisites/linux/windows-taskbar-thinlinc-client.png?width=600px)
 
## Log in to Linux with the ThinLinc Client

Click on the **ThinLinc Client** icon in the **taskbar** to log in to the Linux
system. Enter the following login information.  

- **Server:**  `thinlinc.student.it.uu.se`.
- **Username:** Your **student account username** on the form `abcd1234`.
- **Password:** Your **Password A**.


![ThinLinc client login](/v1/images/prerequisites/linux/thinlinc-login.png?width=400px)

Click **Connect** to login. The first time you log in, you may see 
the following message.

![ThinLinc client Continue/Abort login](/v1/images/prerequisites/linux/trust-this-host.png?width=400px)

If you see the above message, click on **Continue**. 

## The Linux desktop

After a successful login, new window will open with the Linux desktop environment.
You can maximize or resize this window, just like for any other window in
Windows. Thus, in the Linux desktop window in Windows you interact with the
Linux system that is actually running on a remote server. 

![](/v1/images/prerequisites/linux/linux-desktop.png?width=600px)

The department Linux system runs [Ubuntu Server 18.04 LTS][18-04-lts] using the
[Gnome flashback][gnome-flashback] desktop environment with the
[Numix][numix] desktop theme.


[18-04-lts]: http://releases.ubuntu.com/18.04/
[gnome-flashback]: https://linuxconfig.org/ubuntu-20-04-gnome-flashback-desktop-installation
[numix]: https://numixproject.github.io/

## The Gnome terminal

You can open a terminal in two ways:

* From the Applicatinos menu at the top left of the desktop:  **Applications** → **Accessories** → **Terminal**.
* By pressing the keyboard shorcut **CTRL** + **ALT** + **T** .

After a few seconds a new terminal window should open.

![](/v1/images/prerequisites/linux/shell-and-terminal/terminal-1.png)

In the upper left corner of the white area of the terminal window you see
`abcd1234@arrhenius:~$ `. This it the shell prompt with your username on the form
`abcd1234` and the name of the Linux server you are
connected to, in this example `arrhenius`. The shell prompt you see might be
different.

## The shell prompt 

In the above example, the prompt shows the username of the logged in user `abcd1234`
together with the name of the physical Linux server `arrhenius` used. You should
see your own user name. If you are logged into a different physical Linux server
you will also see a different server name in the prompt.

It is also possible to [tweak the prompt][tweak-prompt] to show custom
information such as your username, local time etc.

[tweak-prompt]: https://help.ubuntu.com/community/CustomizingBashPrompt

## No shell prompt in instructions

Since the appearance of the shell prompt might vary, in all further instructions
the shell prompt will be omitted and commands you enter att the shell prompt will be 
presented in a box like this.

``` bash session
ls -F  # Example shell command
```

If you hover over the box above, a copy button {{% icon icon="copy" %}} will appear
in the upper right corner. Press this button to copy the text in the box. Now you
can paste the copied command at the shell prompt in your terminal and press
enter to execute the command. 

## Log out from the Linux system

There are two options for logging out from the Linux system. To log out of Linux you can:

1. Click on the icon with a person walking through a door at the top. 

2. Click on the icon with a computer at the top right and then select **Log Out**
to log out.

![](/v1/images/prerequisites/linux/linux-log-out.png?width=600px)

After you log out, the Linux window closes. If you logged out by mistake
you must [restart the ThinLinc Client and
login](#start-the-thinlinc-client) again to continue working with Linux.

### Remote login to the department Linux system with SSH

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


## Known problems

A list of [know problems with the department Linux
system](http://www.it.uu.se/datordrift/faq/thinlinc) and workarounds.