---
title: 'Recording IPTV Using ffmpeg'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I don't usually watch TV. But from time to time there is something interesting on the programme, such as debates on local politics. Unfortunately, those usually run at a time of day where I'm not able (or more likely not willing) to tune in an pay attention to an hour of political discourse. So I want to record them and watch later instead.

===

My ISP provides its IPTV programme as MPEG-TS streams via multicast UDP. And they even link a M3U playlist of all stations on their website, so you can basically watch TV with any client whatsover, as long as it speaks IGMP and understands MPEG-TS video streams. This makes recording very easy, as this is supported by a lot of multimedia processing software, including ffmpeg.

The playlist consists of a list of TV stations, each of which is represented by its own multicast group and an UDP port. So let's just take the first station and see what ffmpeg finds in there:

>     Input #0, mpegts, from 'udp://239.77.0.77:5000':
>       Duration: N/A, start: 41892.675600, bitrate: N/A
>       Program 9038 
>         Metadata:
>           service_name    : SRF 1 HD
>           service_provider: Schweizer Radio und Fernsehen
>         Stream #0:0[0x50]: Video: h264 (High) ([27][0][0][0] / 0x001B), yuv420p(tv, bt709, progressive), 1280x720 [SAR 1:1 DAR >     16:9], 50 fps, 50 tbr, 90k tbn, 100 tbc
>         Stream #0:1[0x51](deu): Audio: mp2 ([3][0][0][0] / 0x0003), 48000 Hz, stereo, fltp, 192 kb/s (clean effects)
>         Stream #0:2[0x52](eng): Audio: mp2 ([3][0][0][0] / 0x0003), 48000 Hz, stereo, fltp, 192 kb/s (clean effects)
>         Stream #0:3[0x5b](deu): Audio: ac3 ([6][0][0][0] / 0x0006), 48000 Hz, 5.1(side), fltp, 448 kb/s (clean effects)
>         Stream #0:4[0x6e](deu,deu): Subtitle: dvb_teletext ([6][0][0][0] / 0x0006)
>         Stream #0:5[0x70]: Unknown: none ([5][0][0][0] / 0x0005)
>         Stream #0:6[0x72]: Unknown: none ([12][0][0][0] / 0x000C)

We can see that the MPEG-TS stream contains multiple individual streams, which are listed in the output above. Now, I don't know what's up with 0:5 and 0:6, or why ffmpeg doesn't understand them. Anyway, I only need the video and one audio channel. Let's just pick the first two, and record an one hour TV show:

>     ffmpeg -f mpegts -i udp://239.77.0.77:5000 -map 0:0 -map 0:1 -c copy -t 3600 recording.mkv

To break it down:

* `-f mpegts` tells ffmpeg that the input is an MPEG transport stream.
* `-i udp://239.77.0.77:5000` tells ffmpeg to join the specified multicast group and receive the MPEG-TS stream on UDP port 5000.
* `-map 0:0 -map 0:1` only extracts the streams 0:0 (the H.264 video stream) and 0:1 (the German audio channel in MP2)
* `-c copy` causes the input stream to be demuxed only, and the selected streams to be written to the output without CPU-intensive decoding and reencoding.
* `-t 3600` terminates the stream after one hour, when the show is over.
* `recording.mkv` is the output filename. The container format (here MKV) is deduced from the filename.

So the whole ffmpeg command takes the original stream as input, demultiplexes it to get the individual media streams, then throws out all but one video and one audio stream, multiplexes them into a Matroska container, which is then written to disk.

### Source
[Recording IPTV Using ffmpeg](https://s3lph.me/recording-iptv-using-ffmpeg.html)