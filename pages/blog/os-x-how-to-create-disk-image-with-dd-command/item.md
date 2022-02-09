---
title: 'OS X - How To Create Disk Image with dd command'
date: '19:46 18-09-2020'
taxonomy:
    category:
        - osx
        - dd
        - backup
    tag:
        - Macintosh
        - 'OS X'
        - dd
        - Backup
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

How do I create or write to images to disk on Apple Mac OS X (macOS) Unix operating system with dd command?

===

The procedure is as follows:

* Open the Terminal app
* Get disk list with the `diskutil list`
* To create the disk image: `dd if=/dev/DISK of=image.dd bs=512`
* To write the disk image: `dd if=image.dd of=/dev/DISK`

### Mirror from
[CIberciti.biz - https://www.cyberciti.biz/faq/how-to-create-disk-image-on-mac-os-x-with-dd-command/](https://www.cyberciti.biz/faq/how-to-create-disk-image-on-mac-os-x-with-dd-command/)

[Archive.is mirror - https://archive.vn/dS8b0](https://archive.vn/dS8b0)