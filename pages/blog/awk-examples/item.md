---
title: 'AWK examples'
media_order: emily-bernal-r2F5ZIEUPtk-unsplash.jpg
date: '15:45 03-11-2020'
taxonomy:
    category:
        - linux
        - bash
        - awk
    tag:
        - Linux
        - 'Text Processing'
        - Bash
        - AWK
hide_git_sync_repo_link: false
hero_image: emily-bernal-r2F5ZIEUPtk-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Few examples for AWK text processor.

===

### Example 1

#### Input
>     card 0: AudioCard_4888 [USB Audio Device]

#### Output
The card ID (in the example above: `0`.

#### Query
>     echo "$(echo "card 0: AudioCard_4888 [USB Audio Device]" | awk -F":" '{print $1}' | awk -F" " '{print $2}')"



### Example 2

#### Input
>     card 0: AudioCard_4888 [USB Audio Device]

#### Output
The number after the AudioCard text (in the example above: `4888`.

#### Query
>     echo "$(echo "card 0: AudioCard_4888 [USB Audio Device]" | awk -F"_" '{print $2}' | awk -F" " '{print $1}')"







