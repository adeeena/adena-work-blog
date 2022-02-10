---
title: 'Remove unnecessary packages from Raspbian 10 Buster'
media_order: 'Annotation 2020-08-04 101615.png,Annotation 2020-08-04 102148.png,Annotation 2020-08-04 102924.png,Annotation 2020-08-04 103022.png,Screen Shot 2020-08-08 at 11.39.59.png,Screen Shot 2020-08-08 at 11.41.56.png,Screen Shot 2020-08-08 at 11.42.21.png,Screen Shot 2020-08-08 at 11.49.30.png,Screen Shot 2020-08-08 at 11.52.48.png,Screen Shot 2020-08-08 at 12.08.47.png'
date: '08:10 04-08-2020'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Scripts to remove unnecessary packages coming with "Recommended" bundle with Raspbian installer.

===

### TL;DR Do the job in one go

>     sudo apt-get remove bluej geany greenfoot* wolfram-engine \
>      mu-editor nodered scratch* sonic-pi python-sense* thonny smartsim \
>      libreoffice* claws-mail teamviewer-host realvnc-vnc* \
>      python-minecraftpi python3-minecraftpi minecraft-pi python-pygame \
>      code-the-classics -y
>     sudo apt autoremove

### Programming

>     sudo apt-get remove bluej geany greenfoot* wolfram-engine mu-editor \
>      nodered scratch* sonic-pi python-sense* thonny -y
>     sudo apt autoremove

Before:

![](Annotation%202020-08-04%20101615.png)

After:

![](Annotation%202020-08-04%20102148.png)


### Education

>     sudo apt-get remove smartsim -y
>     sudo apt autoremove

Before:

![](Annotation%202020-08-04%20102924.png)

After:

![](Annotation%202020-08-04%20103022.png)

### Office

>     sudo apt-get remove libreoffice* -y
>     sudo apt autoremove     

Before:

![](Screen%20Shot%202020-08-08%20at%2011.39.59.png)

After:

![](Screen%20Shot%202020-08-08%20at%2011.41.56.png)

### Internet

>     sudo apt-get remove claws-mail teamviewer-host realvnc-vnc* -y
>     sudo apt autoremove

Before:

![](Screen%20Shot%202020-08-08%20at%2011.42.21.png)

After:

![](Screen%20Shot%202020-08-08%20at%2011.49.30.png)

### Games

_NOTE:_ The games Boing, Bunner, Cavern, Myriapod are in the `Code the Classics` package.

>     sudo apt-get remove python-minecraftpi python3-minecraftpi \
>      minecraft-pi python-pygame code-the-classics -y
>     sudo apt autoremove

Before:

![](Screen%20Shot%202020-08-08%20at%2011.52.48.png)

After:

![](Screen%20Shot%202020-08-08%20at%2012.08.47.png)
