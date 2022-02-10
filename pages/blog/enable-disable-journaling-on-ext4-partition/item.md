---
title: 'Enable / Disable Journaling on EXT4 partition'
date: '11:44 29-06-2020'
taxonomy:
    category:
        - linux
        - ext4
    tag:
        - Linux
        - Journaling
        - Ext4
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

"^has_journal" switched it off, "has_journal" will switch it back on.
The **tune2fs** command allows you to change options on a file system that already exists.

===

To disable journaling:

    sudo mke2fs -t ext4 -O ^has_journal /dev/sdb1

To (re)enable journaling:

    sudo tune2fs -O has_journal /dev/sdb1

The dumpe2fs command will let you see the state of the file system:

    sudo dumpe2fs -h /dev/sdb1