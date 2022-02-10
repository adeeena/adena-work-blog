---
title: 'How can i identify multiple USB sound cards on Raspberry Pi?'
media_order: 'Screenshot 2020-11-03 162420.png,Screenshot 2020-11-03 162928.png,photo-1604264024773-ca2c2f8ceb82.jpg'
published: true
date: '15:13 03-11-2020'
taxonomy:
    category:
        - linux
        - 'raspberry pi'
        - audio
    tag:
        - 'Raspberry Pi'
        - Linux
        - 'Audio cards'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: photo-1604264024773-ca2c2f8ceb82.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

We would like to stream multiple analogue microphone inputs through Raspberry Pi.

We have a following configuration:

* Raspberry Pi 3B/4
* 2 identical USB audio cards

As the Linux kernel might order the USB cards randomly during bootup, we cannot really rely on the `arecord -l` output. The `ls -la /proc/asound/cards` does not help to distinguish by device ID either.

How to identify USB sound cards to avoid this situation?

===

### Solution

Device path basically defines which USB port the card is plugged into. Run `ls -la /sys/class/sound/card*` to list cards and their device pathes, then write new name to card's id attribute. (Use "device path" to rename each card.)

>     pi@raspberrypi:~$ ls -la /sys/class/sound/card*
>     lrwxrwxrwx 1 root root 0 Oct 12 10:03 /sys/class/sound/card0 -> ../../devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.4/1-1.4:1.0/sound/card0
>     lrwxrwxrwx 1 root root 0 Oct 12 10:03 /sys/class/sound/card1 -> ../../devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.3/1-1.3:1.0/sound/card1
>     lrwxrwxrwx 1 root root 0 Oct 12 10:03 /sys/class/sound/card2 -> ../../devices/platform/soc/soc:audio/sound/card2

![](Screenshot%202020-11-03%20162420.png)

The query gives 3 device paths (with 2 USB audio card inserted, and the integrated chip on the Raspberry's board). Index might be different, but device path won't change until you plug the card into a different USB port.

### Setting new name

Use these device paths to set new name:

>     echo -n AudioCard_4444 > /sys/devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.4/1-1.4:1.0/sound/card*/id
>     echo -n AudioCard_4888 > /sys/devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.3/1-1.3:1.0/sound/card*/id

That will also change names in `cat /proc/asound/cards` and `arecord -l` output.

![](Screenshot%202020-11-03%20162928.png)


### Setting name through udev rule

You can define rules to set those names automatically when devices are detected. For udev write to `/etc/udev/rules.d/85-my-usb-audio.rules` something like:

>     SUBSYSTEM!="sound", GOTO="my_usb_audio_end"
>     
>     ACTION!="add", GOTO="my_usb_audio_end"
>     
>     DEVPATH=="/devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.3/1-1.3:1.0/sound/card?", ATTR{id}="AudioCard_4444"
>     
>     DEVPATH=="/devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.4/1-1.4:1.0/sound/card?", ATTR{id}="AudioCard_4888"
>     
>     LABEL="my_usb_audio_end"


### Sources
[StackOverflow - linux device driver - How can i identify multiple USB sound cards - https://stackoverflow.com/questions/43361613/how-can-i-identify-multiple-usb-sound-cards/44218856#44218856](https://stackoverflow.com/questions/43361613/how-can-i-identify-multiple-usb-sound-cards/44218856#44218856)

[Archive.vn link - https://archive.vn/QWaZ1](https://archive.vn/QWaZ1)