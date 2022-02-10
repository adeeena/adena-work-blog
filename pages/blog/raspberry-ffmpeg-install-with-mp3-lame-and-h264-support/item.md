---
title: 'Raspberry - FFMpeg - Install with MP3 Lame and .h264 support'
date: '13:10 13-08-2020'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

The Raspberry Pi is a fantastic little computer for recording video. For about €50,- you can record in HD with full customizability and for as long as you want or have storage for. However, one issue is that the .h264 container it records in is hard to work with. It is therefore often important to convert videos to widely applicable formats like .mp4 to be able to view them properly and get the right meta information. For this I recommend the program FFmpeg.

Installing ffmpeg on a Raspberry Pi is not as simple as downloading an executable from the command line, but it is also not too difficult.

===

>     sudo apt-get install libmp3lame-dev
>     git clone --depth 1 https://code.videolan.org/videolan/x264
>     cd x264
>     ./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl
>     make -j4
>     sudo make install

### Install ffmpeg with h264 and mp3-lame

>     git clone git://source.ffmpeg.org/ffmpeg --depth=1
>     cd ffmpeg
>     ./configure --arch=armel --target-os=linux --enable-gpl --enable-libx264 --enable-nonfree --enable-libmp3lame
>     make -j4
>     sudo make install


### Convert h264 video
Now you are ready to convert a h264 video on your Raspberry Pi! Simply run the following command:

>     ffmpeg -i USER_VIDEO.h264 -vcodec copy USER_VIDEO.mp4

There are many options available and many other ways to convert h264 videos with ffmpeg, but this command is the quickest of all methods that I tested as it basically just changes the container without re-encoding.

### Mirror from
[JolleJolles - Installing ffmpeg with h264 support on Raspberry Pi - http://jollejolles.com/installing-ffmpeg-with-h264-support-on-raspberry-pi/](http://jollejolles.com/installing-ffmpeg-with-h264-support-on-raspberry-pi/)

[StackOverflow - Installing libmp3lame to work with FFMPEG on raspberry pi - https://stackoverflow.com/questions/40021623/installing-libmp3lame-to-work-with-ffmpeg-on-raspberry-pi](https://stackoverflow.com/questions/40021623/installing-libmp3lame-to-work-with-ffmpeg-on-raspberry-pi)