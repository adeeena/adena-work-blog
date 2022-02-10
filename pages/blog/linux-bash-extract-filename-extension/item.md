---
title: 'Linux Bash - Extract filename and extension'
taxonomy:
    category:
        - linux
        - bash
    tag:
        - Programming
        - Linux
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

I want to get the filename (without extension) and the extension separately.

The best solution I found so far is:

>     NAME=`echo "$FILE" | cut -d'.' -f1`
>     EXTENSION=`echo "$FILE" | cut -d'.' -f2`

This is wrong because it doesn't work if the file name contains multiple `.` characters. If, let's say, I have `a.b.js`, it will consider a and `b.js`, instead of `a.b` and `js`.

It can be easily done in Python with

>     file, ext = os.path.splitext(path)

but I'd prefer not to fire up a Python interpreter just for this, if possible.

Any better ideas?

===

First, get file name without the path:

>     filename=$(basename -- "$fullfile")
>     extension="${filename##*.}"
>     filename="${filename%.*}"

Alternatively, you can focus on the last '/' of the path instead of the '.' which should work even if you have unpredictable file extensions:

>     filename="${fullfile##*/}"

### References
[GNU - Shell Parameter Expansions - http://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html#Shell-Parameter-Expansion](http://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html#Shell-Parameter-Expansion)

### Mirror from

[StackOverflow - Extract filename and extension in Bash
 - https://stackoverflow.com/questions/965053/extract-filename-and-extension-in-bash](https://stackoverflow.com/questions/965053/extract-filename-and-extension-in-bash)