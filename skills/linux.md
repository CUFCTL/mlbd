## Linux

Linux is a family of free and open-source operating systems based around the _Linux kernel_. The history of Linux is quite complex, and so is it's family tree. There are _hundreds_ of Linux flavors available on the Internet now, but for starters here are just a few flavors to be aware of:

#### Debian

The Debain family houses the (arguably) most popular Linux distribution, __Ubuntu__. Ubuntu is the OS of choice for anyone using Linux for the first time, because it has all the features of a typical OS such as Windows and Mac. Other members include __Raspbian__, the OS for Raspberry Pi.

#### Red Hat

Red Hat is the name of both a company and their OS brand. It's members include __Red Hat Enterprise Linux__ and __CentOS__, both of which are used commonly for HPC systems. CentOS is free, RHEL is not; Clemson's Palmetto cluster currently uses RHEL.

#### Arch Linux

Arch Linux is minimal, elegant, fast, and it has a learning curve. Not for the faint of heart, or for beginners. Mostly used by people who want to be able to say that they use Arch Linux.

For people who are new to Linux, it is enough to know that (1) __Ubuntu__ is a good first choice to learn Linux because a lot of people use it so it's well maintained, and (2) RHEL/CentOS is typically used for HPC systems (including Palmetto). Both flavors are fairly similar on the command-line, so if you learn one you can figure out the other pretty quickly.

### Installation (Ubuntu)

There are a lot of ways to get Linux, so we will focus on the most comman ways to use Ubuntu. In particular, you don't have to completely wipe your computer and install Linux from scratch, you can use it alongside Windows or Mac (or whatever you have).

_NOTE: If you only need Linux to access Palmetto through SSH, you may not need to install Ubuntu at all. Mac OS already has a built-in terminal which can use ssh, and Windows 10 has a similar feature which is described below._

#### Ubuntu Bash for Windows 10

For anyone with Windows 10, this option is probably the best if you only need a basic command-line (for example, to access Palmetto). You just have to [tweak some Windows settings](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and then you can install Ubuntu like any other app:

#### Virtual machine

A virtual machine is an entire OS contained in a program, running on another OS. In this case, you can create an Ubuntu virtual machine and run it on Windows or Mac. [VirtualBox](https://www.virtualbox.org/) is a common tool for doing this. While this option is probably the easiest to set up, it's not as efficient because you have to run two OS's at the same time.

#### Dual-boot

The standard way to install Ubuntu is to [download the ISO image](https://www.ubuntu.com/download/desktop), [burn it to a USB drive](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows#6) (don't worry, not permanent), boot into the USB drive (instead of your hard drive like normal), and install Ubuntu using the installation wizard. But Ubuntu is polite, so it will let you split your hard drive in two and install Ubuntu on one half while leaving your original OS on the other half, untouched. This process is called __dual-booting__. It is the recommended practice for anyone who wants to use Linux heavily but also wants to keep Windows or Mac. It is more efficient than a virtual machine, although it introduces a slight inconvencience of having to pick your OS every time you boot your computer.

### Tutorials

Honestly, there are so many Linux tutorials already... just find one of them and go with it. You will most likely learn how to use Linux by using it and looking up help when you need it. Google and Stack Overflow are your friends.
