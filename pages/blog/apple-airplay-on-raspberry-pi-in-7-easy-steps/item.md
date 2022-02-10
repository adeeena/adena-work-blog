---
title: 'Apple Airplay on Raspberry Pi in 7 Easy Steps'
media_order: 'select-airplay.jpg,dynamic-wang-efMMsZP8Qqw-unsplash.jpg'
date: '20:44 13-03-2021'
taxonomy:
    category:
        - 'Raspberry PI'
        - Shairport
        - AirPlay
    tag:
        - 'Raspberry Pi'
        - Apple
        - AirPlay
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: dynamic-wang-efMMsZP8Qqw-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Do you have an existing hifi system that’s too old to support Apple Airplay? Don’t want to replace it with lower-quality wireless speakers, but want to stream music? It’s a common problem. The good news is, for the cost of a Raspberry Pi you can build an Airplay server so you can stream music directly from an iPhone or iPad to your hifi system.

This guide shows you how to set up `Shairport-sync` so you’ll have a Raspberry Pi playing music  in 7 easy steps.

===

### Before You Start
Before you can set up a Raspberry Pi as an Airplay server there are a few things you’ll need.

### Choosing a Raspberry Pi
You may have one lying around already. Most models should work, but the Pi Zero will be a problem because it’s harder to set up a network on the Zero. I suggest googling to find out how to do that before you start. Things will be far easier with a Raspberry Pi Zero W model, and the single-core CPU in there works pretty well for streaming so long as you don’t have anything other heavy-duty stuff running.

If you’re buying new, I’d suggest a Model 3. I used a Model 3 A+ with no problems, because I wanted a smaller form factor, wifi and enough grunt. The Raspberry Pi 3 Model B will work equally well.

### Choosing an Operating System
The first thing you’ll need is an SD card with an appropriate operating system. You won’t need much storage; you should be able to get away with 4GB, but nowadays it’s hard to find such small cards and you’d be better off buying a multi-pack of 32GB class 10 microSD cards so you can use them for other projects.

When choosing the OS, these days there are a lot of different options, from “traditional” Raspbian to dedicated media operating systems, and even Windows 10 IoT.

Now, an Airplay server is not like Kodi which requires a rich graphical interface. It’s absolutely possible to run it with no UI at all, since all of the user interaction will be done on the iOS or macOS device.

It might be nice to have some kind of display showing the current artist and track information, [but that’s a project in itself](https://appcodelabs.com/show-artist-song-metadata-using-airplay-on-raspberry-pi). For now, concentrate on getting the server working!

So with that established you should be looking towards a more barebones system. There’s no need for lots of applications, and for glitch-free operation you want as few processes running as possible. This is to leave as much CPU and RAM as you can for the media streaming application.

One problem is that some of the barebones OSes, such as Arch Linux, are are aimed at people with quite a lot of Linux knowledge. They’re quite cutting edge and you’re more likely to run into issues when installing third party applications and libraries.

At the other end of the spectrum there are some distributions which are dedicated to this task, and should work out of the box, but you won’t learn anything from that.

For those reasons, if this is your first installation of an Airplay device, I recommend choosing Raspbian Lite. It is mature because it’s derived from the original Raspbian OS, but it has had the GUI removed to save resources. It’s entirely command-line driven, and can easily configured to run in “headless” mode, i.e. accessed entirely remotely over the network with no need for a keyboard or monitor. This makes it ideal for putting into an appliance, and hopefully you’ll learn something in the process.

### Install Raspbian Lite and Enable SSH
If you already have an installation, or are confident in doing that part yourself, go ahead.

Otherwise follow my tutorial below, which takes you step-by-step from a raw Raspberry Pi to SSH-enabled Raspbian Lite:-

[How To Set Up a Raspberry Pi You Can Control From Anywhere in 30 Minutes](https://appcodelabs.com/build-a-minimal-raspberry-pi-installation-for-headless-mode-operation)

Once you’re set up, you should be able to login to the Raspberry Pi remotely from another computer on your network, and your Pi should have internet access.

### Configure the Airplay Server
1. Install Dependencies

First off, you’ll need to install some dependencies so you can build the Airplay server application. Run the following:-

>     sudo apt-get update
>     sudo apt-get install autoconf automake avahi-daemon build-essential \
>          git libasound2-dev libavahi-client-dev libconfig-dev libdaemon-dev \
>           libpopt-dev libssl-dev libtool xmltoman

2. Build & Install `shairport-sync`

`shairport-sync` is a fantastic piece of software maintained by [Mike Brady](https://github.com/mikebrady). It turns your Linux machine into an Apple Airplay server. One of the best things about it is that it runs entirely on the command line, and while it has a million configuration options, it’s surprisingly easy to get working out of the box.

First grab it from github:-

>     git clone https://github.com/mikebrady/shairport-sync.git

Now navigate to the `shairport-sync` directory and configure the build:-

>     cd shairport-sync
>     autoreconf -i -f
>     ./configure --with-alsa --with-avahi --with-ssl=openssl --with-systemd --with-metadata

Finally build and install the application:-

>     make
>     sudo make install

At the end of this procedure you should have a working installation of `shairport-sync`.

3. Configure the Audio Output

You’re now ready to test the Airplay audio. First you’ll need some hardware. Any of the following will work:-

* headphones, or just about any old earbuds with a 3mm jack
* active speakers from a desktop PC
* or if you’re serious, a hi-fi amp with a cable that converts 3.5mm jack to a pair of RCA phono plugs

Now you need to configure the audio path on the Raspberry Pi. It’s generally set to “auto” but you need to force it to go to the 3.5mm jack. Run raspi-config:-

>     sudo raspi-config

Select `“7. Advanced Options”`, then `“A4. Audio”`, then choose Option 1 `“Force 3.5mm (‘headphone’) jack”`. This will force the audio path to the 3.5mm headphone jack.

4. Set the Volume
The volume can tend to be very low, so change it to max using:-

>     amixer sset PCM,0 100%

The volume setting is a bit difficult to use because it’s configured in dB (decibels), which if you’re not an engineer are very unintuitive. In decibels, full volume is generally 0dB, and zero volume is around -100dB. So as if that wasn’t bad enough – and this is the main brain-twister – the decibel scale is not linear.

So when you use the % notation with the amixer command above, you might think it works like a normal volume control but it does not. If you want the volume a touch lower you might perfectly reasonably change it to 80%:-

>     amixer sset PCM,0 80%

You’ll see the output reports this as around -17dB, but crucially you’ll notice it’s barely audible. The percentage is directly converted to decibels, with the upshot that you can only really hear anything above around 70%.

So, TLDR: keep the volume set to 100%.

5. Test Airplay to the Raspberry Pi

Now start shairport-sync with:-

>     sudo service shairport-sync start

Nothing’s going to happen until you start Airplaying to it, so grab an iPhone or something that supports Airplay, and ensure it’s on the same network as the Raspberry Pi. Start playing some music, and from the Airplay icon select “raspberrypi”  and then “Done”.

![](select-airplay.jpg)

If you can’t hear anything, Turn your iPhone (or whatever source you’re using) volume up high, because the PCM (headphone) output on the Raspberry Pi is not great.

At this point you should hear the music played through the Raspberry Pi!

6. Configure `shairport-sync` to Start Automatically

Obviously in a dedicated media player you don’t want to have to start services manually: you want `shairport-sync` to run as soon as the Pi has booted. Luckily we configured it for systemd operation, which means we can easily enable the service to automatically launch. Just input:-

>     sudo systemctl enable shairport-sync

This will output a message similar to this:-

>     Created symlink /etc/systemd/system/multi-user.target.wants
>       /shairport-sync.service → /lib/systemd/system/shairport-sync.service.

That’s all you need to do to create a persistent Airplay server. Reboot:-

>     sudo reboot

When you log back in over SSH, you can query the shairport-sync service like this:-

>     sudo systemctl status shairport-sync.service

which will (hopefully) produce something like:-

>     ● shairport-sync.service - Shairport Sync - AirPlay Audio Receiver
>        Loaded: loaded (/lib/systemd/system/shairport-sync.service; enabled; vendor preset: enabled)
>        Active: active (running) since Wed 2018-12-05 22:30:26 GMT; 1min 16s ago
>     Main PID: 515 (shairport-sync)
>        CGroup: /system.slice/shairport-sync.service
>                └─515 /usr/local/bin/shairport-sync
>     
>     Dec 05 22:30:26 raspberrypi systemd[1]: Started Shairport Sync - AirPlay Audio Receiver.

So you should be able to Airplay to the Raspberry Pi now every time you boot it up.

7. Prevent Wifi Dropouts
The Raspberry Pi wifi will tend to go into power-saving mode periodically, which can cause serious audio glitching when using Airplay. You can prevent this by adding a line to the file  `/etc/network/interfaces` . Edit the file using:-

>     sudo nano /etc/network/interfaces

Go to the end of the file and add the lines:-

>     # Disable wifi power management
>     wireless-power off

After all this, reboot:-

>     sudo reboot

Now you should be up and running!

### Troubleshooting
If you can’t see the raspberrypi device listed in Airplay, check the shairport-sync service is running with:-

>     sudo systemctl status shairport-sync.service

Check it’s on the same LAN as your Airplay source device, and preferably on the same Wifi network, and in range.

If you’re connected to Airplay and you can’t hear any audio, check the volume levels are high enough.

On the source device, just slide the volume up to about 80-90%.
For the Raspberry Pi PCM audio, check the volume setting using:-

>     amixer sget PCM

This will output the current setting like this:-

>     Mono: Playback 400 [100%] [4.00dB] [on]

If you’re seeing anything below 70% (-27dB) then you probably won’t be able to hear it. It’s best just to keep this setting at 100%.

Obviously check your audio hardware too!

### Note on Audio Quality

This article gained a lot of interest after featuring on Hacker News, and many people mentioned the poor audio quality from the Raspberry Pi’s built-in audio jack. I was planning on writing a follow-up piece about using a DAC board to improve the audio, but before I get around to that I should probably mention it here.

I have tried two DAC boards. The first was the IQAudIO PiDAC+, which has a perfect form factor to fit the new Pi 3 Model A+. I have also used the HiFiBerry DAC+ Zero on a Pi Zero W. They’re both incredibly simple to set up and they sound great.

Another very popular one that I haven’t personally used (yet) is the Pimoroni pHAT DAC.

I have yet to do serious comparative tests on them, but choose either and you’ll see a huge improvement in audio quality.

### How To Display Artist & Song Info

When you configured the `shairport-sync` code earlier, you might have noticed the `--with-metadata` option. Including that option means you can access the metadata of the currently playing track, in other words the artist, album and song information. Making sense of this information is beyond the scope of this article, but I have written another article showing how you can show it on a display.

Thank you!

### Source
[Apple Airplay on Raspberry Pi in 7 Easy Steps](https://appcodelabs.com/7-easy-steps-to-apple-airplay-on-raspberry-pi)

[Archive - https://archive.ph/Csg2b](https://archive.ph/Csg2b)