---
title: 'Linux Bash - Adding timestamp to a filename with mv in BASH'
date: '12:23 24-07-2020'
taxonomy:
    category:
        - linux
        - bash
        - timestamp
    tag:
        - Linux
        - Programming
        - Bash
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I've got a program that adds to a log file while it's running. Over time that log file gets huge. I'd like to create a startup script which will rename and move the log file before each run, effectively creating separate log files for each run of the program.

===

>     mv server.log logs/$(date -d "today" +"%Y%m%d%H%M").log

There **must not** be a space between the plus sign and the double quotes.

### Mirror from

[StackOverflow - Adding timestamp to a filename with mv in BASH - https://stackoverflow.com/questions/8228047/adding-timestamp-to-a-filename-with-mv-in-bash/8229220#8229220](https://stackoverflow.com/questions/8228047/adding-timestamp-to-a-filename-with-mv-in-bash/8229220#8229220)