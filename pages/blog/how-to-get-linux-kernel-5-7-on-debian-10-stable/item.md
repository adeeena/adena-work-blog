---
title: 'How to get Linux kernel 5.7 on Debian 10 Stable'
media_order: 'Screen Shot 2020-08-15 at 17.13.46.png,Screen Shot 2020-08-15 at 17.18.54.png'
taxonomy:
    category:
        - linux-kernel-5.7
        - enable-debian-backports
    tag:
        - Linux
        - Debian
        - 'Kernel Update'
        - 'Linux 5.7 Kernel'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Debian Linux is known for being stable and unchanging. For most, this philosophy is pretty great. Everything works well, and nothing breaks down. However, this stability comes at a cost. Often, users are stuck on an old, outdated Linux kernel.

===

### Enabling Debian Backports for Debian 10 Stable
Debian 10, the latest release from the Debian project, comes with Linux kernel version 4.19. It is the latest long term supported release of the Linux kernel and the most recent version of the Linux kernel that Debian 10 Stable users can get their hands on. If you’d like to get version 5.7, which is (as of writing this) the most recent stable release of the Linux kernel, you will need to set up the Debian Backports repository on the system.

Enabling Debian Backports means editing the `sources.list` file which is located in the `/etc/apt/` directory. As a result, it is a good idea to create a backup of this file before continuing. Using the `cp` command, make a copy of `sources.list.`

>     sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

With the backup created, it is time to start the editing process. Using the Nano text editor, load up the `sources.list` file. Be sure to use `sudo`, or if you do not have `sudo` set up on Debian, use `su`.

>     sudo nano -w /etc/apt/sources.list

Or, for non-sudo users.

>     su -
>     nano -w /etc/apt/sources.list

Inside of Nano, make your way to the bottom by pressing the Down Arrow on the keyboard. Then, write out the following sentence.

>     # Debian Buster Backports.

Next, press the Enter key on the keyboard to create a new line in the file. Then, add in the new backports repo for Debian 10 Buster.

>     deb http://deb.debian.org/debian buster-backports main

After adding the repository line, it is time to save Nano. Press Ctrl + O to save your edits. Close the Nano text editor with Ctrl + X.


Finally, once out of the Nano text editor, use the apt-get update command to finish setting up Debian Backports.

>     sudo apt update


### Finding kernel 5.7
Now that Debian Backports is enabled on the system, you need to locate Linux kernel version 5.7 in the software repositories. To do this, use the apt search command and search for the package linux-image.

Note: if you need to install custom kernel modules, you must also search for and install the package linux-headers.

>     apt search linux-image

After running the command above, you should see a print-out in the terminal outlining various versions of the Linux kernel available for Debian 10 Stable.

Alternatively, if this print-out is too tricky to sort through, run apt search with the grep command, and filter `buster-backports`.



>     apt search linux-image | grep buster-backports

![](Screen%20Shot%202020-08-15%20at%2017.13.46.png)

Inside of the search results, two variations of Linux Kernel 5.7 are present. These variations are:

>     linux-image-5.7.0-0.bpo.2-amd64

>     linux-image-5.7.0-0.bpo.2-cloud-amd64

If you are running Debian 10 Stable on a desktop or laptop computer, you will need to install the `linux-image-5.7.0-0.bpo.2-amd64` package, as it includes all of the various desktop Linux drivers required to run on a traditional computer system.

Alternatively, if you use Debian Linux on a server, you have a choice. For a `cloud-centric` kernel, feel free to install `linux-image-5.7.0-0.bpo.2-cloud`. Or, install the desktop kernel (`linux-image-5.7.0-0.bpo-2`).

### Installing Kernel 5.7 on Debian 10 Stable
Since we’ve enabled the Debian Backports repository on the system, there are no special hoops to jump through to get Linux Kernel 5.7 up and running. Instead, we can install everything right from the software repositories. Follow the instructions below to get 5.7 up and running on your Debian 10 system.

#### Debian Desktop Installation

Using the `apt` command in the terminal, load up Linux Kernel 5.7 on your Debian 10 Stable desktop.

>     sudo apt install linux-image-5.7.0-0.bpo.2-amd64

Additionally, be sure to install the 5.7 Linux Kernel headers, if you rely on modules.

>     sudo apt install linux-headers-5.7.0-0.bpo.2-amd64

#### Debian Server installation
Need to get Linux Kernel 5.7 working on Debian Server? Do the following.

First, determine whether you need the 5.7 cloud kernel or the 5.7 desktop kernel. Then, use the `apt install` command to load it up on the system.

>     sudo apt install linux-image-5.7.0-0.bpo.2-cloud-amd64

Or

>     sudo apt install linux-image-5.7.0-0.bpo.2-amd64

Be sure also to load up the Linux Headers if you require custom modules:

>     sudo apt install linux-headers-5.7.0-0.bpo.2-cloud-amd64

or

>     sudo apt install linux-headers-5.7.0-0.bpo.2-amd64

Once the installation process is complete, reboot your Debian 10 Stable system. Upon logging back in, open up a terminal window and use the uname command. It will confirm you are using Linux Kernel 5.7.



>     uname -a


![](Screen%20Shot%202020-08-15%20at%2017.18.54.png)


### Source
[AddictiveTips - How to get Linux kernel 5.3 on Debian 10 Stable - https://www.addictivetips.com/ubuntu-linux-tips/get-linux-kernel-5-3-on-debian-10-stable/](https://www.addictivetips.com/ubuntu-linux-tips/get-linux-kernel-5-3-on-debian-10-stable/)