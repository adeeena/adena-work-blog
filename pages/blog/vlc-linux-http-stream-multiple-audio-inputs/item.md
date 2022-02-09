---
title: 'VLC Linux - HTTP stream multiple audio inputs, one by one'
media_order: 'aplay.png,julian-hanslmaier-33xjcVqOPvM-unsplash.jpg'
date: '15:17 30-10-2020'
taxonomy:
    category:
        - VLC
        - linux
        - 'Console VLC'
        - Pi
        - 'Raspberry PI'
        - Audio
    tag:
        - vlc
        - linux
        - 'Raspberry Pi'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: julian-hanslmaier-33xjcVqOPvM-unsplash.jpg
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
* 2 or more identical USB audio cards

How to create script to start HTTP streaming audio inputs when available?

===

### Pre-requistiques
Audio cards are distinguished, as described in [this blog post](https://pegazus.space/blog/how-can-i-identify-multiple-usb-sound-cards-on-raspberry-pi), [backup link](https://archive.vn/vrJal).

### Script

>     #!/bin/sh
>     
>     HARDWARE_SUBADDRESS=",0"
>     STREAM_LIST_FILE="/home/pi/streams.log"
>     echo "" > "$STREAM_LIST_FILE"
>     
>     arecord -l | grep "AudioCard" > "/home/pi/audiocards.log"
>     
>     arecord -l | grep "AudioCard" | while read -r d ; do
>       # Example:
>       # card 0: AudioCard_4888 [USB Audio Device]
>       # this variable retrieves card id
>       DEVICE_ID="$(echo "$d" | awk -F":" '{print $1}' | awk -F" " '{print $2}')"
>       # this retrieves the number after "AudioCard" for identifying the port to create the http stream to.
>       DESIRED_PORT="$(echo "$d" | awk -F"_" '{print $2}' | awk -F" " '{print $1}')"
>     
>       HARDWARE_ADDRESS=$(echo $DEVICE_ID$HARDWARE_SUBADDRESS)
>     
>       cvlc -vvv alsa://hw:$HARDWARE_ADDRESS --sout \
>         '#transcode{acodec=mp3,ab=128,channels=2,samplerate=44100}:standard{access=http,dst=0.0.0.0:'"$DESIRED_PORT"'/stream.mp3}' &
>     
>       echo "0.0.0.0:$DESIRED_PORT/stream.mp3" >> "$STREAM_LIST_FILE"
>     done
>     

### Result
As many HTTP MP3 streams on desired ports as descibed by udev and as many audio cards are available.