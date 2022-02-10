---
title: 'Raspberry - Watchdog for Raspbian Stretch 10'
date: '11:47 08-08-2020'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

A watchdog timer is a special kind of timer commonly found on embedded systems that is used to detect when the running software is hung up on some task. The watchdog timer is basically a countdown timer that counts from some initial value down to zero. When zero is reached, the watchdog timer understands that the system is hung up and resets it. Therefore, the running software must periodically update the watchdog timer with a new value to stop it from reaching zero and causing a reset. When the running software is locked up doing a certain task and cannot update the watchdog timer, the timer will inevitably reach zero and a reset will occur.

Luckily for us, the `Broadcom BCM2835 SoC` on the Raspberry Pi comes with a hardware-based watchdog timer that can do just that. You will find this specially useful if you have a Raspberry Pi in a remote location and the operating system hangs and there's no one around to reboot it.

===

### Installing the Watchdog
>     sudo apt-get install watchdog
>     sudo update-rc.d watchdog enable

### Watchdog configuration
>     sudo apt-get install watchdog chkconfig
>     sudo chkconfig watchdog on
>     sudo /etc/init.d/watchdog start

Then open the configuration file:

>     sudo nano /etc/watchdog.conf

Uncomment the following lines:

>     watchdog-device = /dev/watchdog
>     interval = 4
>     realtime = yes
>     priority = 1

Save it. In the file `/etc/systemd/system.conf`, uncomment this line:

>     RuntimeWatchdogSec=14

### Kernel configuration

Create the following new file:

>     sudo nano /etc/modprobe.d/bcm2835_wdt.conf

Add these new lines:

>     alias char-major-10-130 bcm2835_wdt
>     alias char-major-10-131 bcm2835_wdt

Save this file. Then, in the file `/etc/modules`, add this line:

>     bcm2835_wdt

And start the new kernel probe:

>     sudo modprobe bcm2835_wdt
>     sudo service watchdog restart

### Testing the configuration

Start the following [forkbomb](https://en.wikipedia.org/wiki/Fork_bomb) to check if the watchdog gets launched:

>     :(){ :|:& };:

The raspberry should eventually restart in 3-4 minutes at most.

### Sources
* [http://www.switchdoc.com/2014/11/reliable-projects-using-internal-watchdog-timer-raspberry-pi/](http://www.switchdoc.com/2014/11/reliable-projects-using-internal-watchdog-timer-raspberry-pi/)
* [http://binerry.de/post/28263824530/raspberry-pi-watchdog-timer](http://binerry.de/post/28263824530/raspberry-pi-watchdog-timer)
* [http://www.megaleecher.net/Watchdog_for_Raspberry_Pi#axzz4MyO5x2gT](http://www.megaleecher.net/Watchdog_for_Raspberry_Pi#axzz4MyO5x2gT)
* [http://www.domoticz.com/wiki/Setting_up_the_raspberry_pi_watchdog](http://www.domoticz.com/wiki/Setting_up_the_raspberry_pi_watchdog)
* [https://github.com/notro/rpi-firmware/wiki/BCM2708vsBCM2835](https://github.com/notro/rpi-firmware/wiki/BCM2708vsBCM2835)

#### BCM2835 data sheets
* [https://www.raspberrypi.org/wp-content/uploads/2012/02/BCM2835-ARM-Peripherals.pdf](https://www.raspberrypi.org/wp-content/uploads/2012/02/BCM2835-ARM-Peripherals.pdf)
* [http://elinux.org/BCM2835_datasheet_errata](http://elinux.org/BCM2835_datasheet_errata)

#### ARM Processor data sheet
* [http://infocenter.arm.com/help/topic/com.arm.doc.ddi0301h/DDI0301H_arm1176jzfs_r0p7_trm.pdf](http://infocenter.arm.com/help/topic/com.arm.doc.ddi0301h/DDI0301H_arm1176jzfs_r0p7_trm.pdf)


### Mirror from
[Framboise314 - Watchdog pour mon Raspberry Pi - https://www.framboise314.fr/watchdog-pour-mon-raspberry-pi/](https://www.framboise314.fr/watchdog-pour-mon-raspberry-pi/)