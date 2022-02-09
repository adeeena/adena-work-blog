---
title: 'How to check CentOS version'
media_order: 'Annotation 2020-07-23 143250.png,Annotation 2020-07-23 143401.png,Annotation 2020-07-23 144742.png,Annotation 2020-07-23 150205.png'
date: '12:29 23-07-2020'
taxonomy:
    category:
        - linux
        - centos
        - 'get vesion'
    tag:
        - Linux
        - CentOS
        - 'Get Version'
        - 'Get Release'
        - 'CentOS 6'
        - 'CentOS 7'
        - 'CentOS 8'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

There are several ways on how to check what version of CentOS is running on your system. The simplest way to check for the CentOS version number is to execute the `cat /etc/centos-release` command. Identifying the accurate CentOS version may be required to help you or your support team to troubleshoot your CentOS system.

===

### Generic commands

#### rpm -q centos-release
>     $ rpm -q centos-release

CentOS version valid for CentOS 6 and higher. Causes to reveal major, minor and asynchronous CentOS version.
![](Annotation%202020-07-23%20143250.png)

#### lsb_release -d
$ lsb_release -d

Requires redhat-lsb package to be installed before execution.

#### rpm -E %{rhel}
$ rpm -E %{rhel}

RPM macro to reveal a major CentOS version

![](Annotation%202020-07-23%20143401.png)

#### rpm --eval %{centos_ver}
>     $ rpm --eval %{centos_ver}

RPM macro to display a major version of CentOS

![](Annotation%202020-07-23%20144742.png)

#### cat /etc/centos-release
>     $ cat /etc/centos-release

Linux cat command to output content of the /etc/centos-release to query CentOS version. Works with CentOS 6 and higher.

![](Annotation%202020-07-23%20150205.png)

### Alternative commands to check CentOS version

In case the above-provided commands did not help you to obtain the CentOS version number you may try the following alternative commands.

Although available only for CentOS version 7 and above the `hostnamectl` command might provide you with a significant clue about your OS version number:

>     $ hostnamectl 
>        Static hostname: localhost.localdomain
>              Icon name: computer-vm
>                Chassis: vm
>             Machine ID: fe069af6a1764e07be909d7cf64add99
>                Boot ID: b81bb73dc549484c8927e830e149eb55
>         Virtualization: kvm
>       Operating System: CentOS Linux 7 (Core)
>            CPE OS Name: cpe:/o:centos:centos:7
>                 Kernel: Linux 3.10.0-862.6.3.el7.x86_64
>           Architecture: x86-64

For more answers try to query all release files within the `/etc` directory:

>     $ cat /etc/*elease
>     CentOS Linux release 7.5.1804 (Core) 
>     NAME="CentOS Linux"
>     VERSION="7 (Core)"
>     ID="centos"
>     ID_LIKE="rhel fedora"
>     VERSION_ID="7"
>     PRETTY_NAME="CentOS Linux 7 (Core)"
>     ANSI_COLOR="0;31"
>     CPE_NAME="cpe:/o:centos:centos:7"
>     HOME_URL="https://www.centos.org/"
>     BUG_REPORT_URL="https://bugs.centos.org/"
>     
>     CENTOS_MANTISBT_PROJECT="CentOS-7"
>     CENTOS_MANTISBT_PROJECT_VERSION="7"
>     REDHAT_SUPPORT_PRODUCT="centos"
>     REDHAT_SUPPORT_PRODUCT_VERSION="7"
>     
>     CentOS Linux release 7.5.1804 (Core) 
>     CentOS Linux release 7.5.1804 (Core)
     

### Bash script to check CentOS version

The following bash script can be used to obtain the CentOS version number given that the `/etc/centos-release` file exists and is populated.

The below script serves as an example, feel free to modify wherever appropriate.

>     #!/bin/bash
>     
>     full=`cat /etc/centos-release | tr -dc '0-9.'`
>     major=$(cat /etc/centos-release | tr -dc '0-9.'|cut -d \. -f1)
>     minor=$(cat /etc/centos-release | tr -dc '0-9.'|cut -d \. -f2)
>     asynchronous=$(cat /etc/centos-release | tr -dc '0-9.'|cut -d \. -f3)
>     
>     echo CentOS Version: $full
>     echo Major Relase: $major
>     echo Minor Relase: $minor
>     echo Asynchronous Relase: $asynchronous
     
Output:

>     $ ./check-centos-version.sh 
>     CentOS Version: 7.5.1804
>     Major Relase: 7
>     Minor Relase: 5
>     Asynchronous Relase: 1804
     

### Python program to check CentOS version

The following python script will output the distribution name along with the OS version number:

>     #!/usr/bin/python
>     
>     import platform
>     print platform.linux_distribution()

Alternatively, one can execute python code directly from the shell:

>     $ python -c 'import platform; print platform.linux_distribution()'

Output:

>     $ python check-centos-version.py 
>     ('CentOS Linux', '7.5.1804', 'Core')
     

### Mirror from
[LinuxConfig - How to check CentOS version - http://linuxconfig.org/how-to-check-centos-version](http://linuxconfig.org/how-to-check-centos-version)