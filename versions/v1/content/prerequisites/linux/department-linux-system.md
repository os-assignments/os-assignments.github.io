---
title: The department Linux system
weight: 10
---

Here you will find information on how to log in to the University's Linux system
from one of the computer rooms for students on campus Ångströmlaboratoriet

## Windows in all computer rooms

In all computer rooms on campus Ångstrumslaboratoriet you find computers with
Windows. 

![Computer screen, keyboard and mouse](/v1/images/module-0/linux/hus-10-pc.jpg?width=400px)

## Linux med ThinLinc

On campus Ångströmlaboratoiet there are a number of large computers ([servers][server]) running
Linux somewhere in a basement. As a student, you will never physically interact
with these server computers.  Instead, you log in to these servers from Windows
to get accerss to the Linux desktop environment on the same screen as Windows. The system used
for this is called [ThinLinc][thinlinc].


[server]: https://en.wikipedia.org/wiki/Server_(computing)

[thinlinc]: https://en.wikipedia.org/wiki/ThinLinc

- To access the Linux system, you must first log into Windows and from
Windows use ThinLinc Client to log in to a Linux server.

## Log in to Windows

To log in to Windows, enter the username of your **student account** on
the form `abcd1234` and **Password A**.

![Windows login](/v1/images/module-0/linux/windows-10-login.jpg?width=400px)

## Software Center (ZENworks)

Somewhere on the Windows desktop you will find **Software center (Zenworks)**.


![Software Center (Zenworks) icon on the Windows desktop](/v1/images/module-0/linux/software-center-icon.png?width=400px)

Double-click the **Software center (Zenworks)** icon. Now a new windows opens with
available software.

![Software center (Zenworks)](/v1/images/module-0/linux/software-center.png?width=400px)

The software you find here may already be installed on the computer.
If a program is not already installed, it will be installed the first time you
trying to use the program.

## Start the ThinLinc client


From **Software Center**, find the  **ThinLinc client**.

![ThinLinc Client icon](/v1/images/module-0/linux/software-center-thinlinc-client-icon.png?width=600px)

Double-click on **ThinLinc Client**. If ThinLinc Client is already installed
, it will start quite quickly. If not installed
already, ThinLInc Client will first be installed and then started. 

## The Windows taskbar

Once the ThinLinc client started, the ThinLinc client icon will appear in the
Windows **taskbar**.

![ThinLinc icon in the Windows taskbar](/v1/images/module-0/linux/windows-taskbar-thinlinc-client.png?width=600px)
 
## Log in to Linux with the ThinLinc Client

Click on the **ThinLinc Client** icon in the **taskbar** to log in to the Linux
system. Enter the following login information.  

- **Server:**  `thinlinc.student.it.uu.se`.
- **Username:** Your **student account username** on the form `abcd1234`.
- **Password:** Your **Password A**.


![ThinLinc client login](/v1/images/module-0/linux/thinlinc-login.png?width=400px)

Click **Connect** to login. The first time you log in, you may see 
the following message.

![ThinLinc client Continue/Abort login](/v1/images/module-0/linux/trust-this-host.png?width=400px)

If you see the above message, click on **Continue**. 

## The Linux desktop

After a successful login, new window will open with the Linux desktop environment.
You can maximize or resize this window, just like for any other window in
Windows. Thus, in the Linux desktop window in Windows you interact with the
Linux system that is actually running on a remote server. 

![](/v1/images/module-0/linux/linux-desktop.png?width=600px)

The department Linux system runs [Ubuntu Server 18.04 LTS][18-04-lts] using the
[Gnome flashback][gnome-flashback] desktop environment with the
[Numix][numix] desktop theme.


[18-04-lts]: http://releases.ubuntu.com/18.04/
[gnome-flashback]: https://linuxconfig.org/ubuntu-20-04-gnome-flashback-desktop-installation
[numix]: https://numixproject.github.io/

## Remote login with SSH

You can access the department Linux [student servers][linux-hosts] remotely
using [SSH][ssh-wp]. In the below example, a user with user name `abcd1234` uses
the `ssh` command from the terminal to log in to the department Linux server
`trygger.it.uu.se`.


[linux-hosts]: http://www.it.uu.se/datordrift/maskinpark/linux

[ssh-wp]: https://en.wikipedia.org/wiki/SSH_(Secure_Shell)

``` shell
$ ssh abcd1234@trygger.it.uu.se
```

## Known problems

A list of [know problems with the department Linux
system](http://www.it.uu.se/datordrift/faq/thinlinc) and workarounds.

## Log out from the Linux system

There are two options for logging out from the Linux system. To log out of Linux you can:

1. Click on the icon with a person walking through a door at the top. 

2. Click on the icon with a computer at the top right and then select **Log Out**
to log out.

![](/v1/images/module-0/linux/linux-log-out.png?width=600px)

After you log out, the Linux window closes. If you logged out by mistake
you must [restart the ThinLinc Client and
login](#start-the-thinlinc-client) again to continue working with Linux.