---
title: 'Linux Bash - How to delete broken symlinks in one go?'
date: '19:08 04-08-2020'
taxonomy:
    category:
        - linux
        - bash
    tag:
        - Linux
        - Bash
        - symlinks
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

How do I get rid of all the broken symlinks I just created in a single fell swoop?

===

This simple one liner does the job quite fast, requires GNU find:

>     find . -xtype l -delete

A bit of explanation:

`-xtype l` tests for links that are broken (it is the opposite of `-type`)

`-delete` deletes the files directly, no need for further bothering with `xargs` or `-exec`

NOTE: `-xtype l` means `-xtype low case L`.

### Mirror from

[Unix/Linux StackExchange - bash- How to delete broken symlinks in one go?](https://unix.stackexchange.com/questions/314974/how-to-delete-broken-symlinks-in-one-go)