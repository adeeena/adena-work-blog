---
title: 'FFMpeg - How to create a thumbnail from the middle of a video?'
date: '12:20 13-08-2020'
taxonomy:
    category:
        - ffmpeg
    tag:
        - ffmpeg
        - Linux
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Is there a way to grab a video thumbnail in FFmpeg?

I'd like to grab the middle-most frame as the video and use that as the thumbnail. Video duration is unknown.

===

>     ffmpeg -ss `ffmpeg -i input.mp4 2>&1 | grep Duration | awk '{print $2}' | tr -d , | awk -F ':' \
>      '{print ($3+$2*60+$1*3600)/2}'` -i input.mp4  -y -f image2 -vcodec mjpeg -vframes 1  output.jpg

### Mirror from
[SuperUser - How to create a thumbnail from the middle of a video with FFmpeg - https://superuser.com/questions/477340/how-to-create-a-thumbnail-from-the-middle-of-a-video-with-ffmpeg](https://superuser.com/questions/477340/how-to-create-a-thumbnail-from-the-middle-of-a-video-with-ffmpeg)