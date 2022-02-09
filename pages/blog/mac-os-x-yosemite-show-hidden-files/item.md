---
title: 'Mac OS X Yosemite - Show hidden files'
media_order: eberhard-grossgasteiger-deQe17KA95s-unsplash.jpg
date: '19:49 13-03-2021'
taxonomy:
    category:
        - osx
        - yosemite
    tag:
        - Macintosh
        - 'OS X'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: eberhard-grossgasteiger-deQe17KA95s-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

===

To enable hidden files/folders in finder windows:

* Open Finder
* Open the Utilities folder
* Open a terminal window
* Copy and paste the following line in:

>     defaults write com.apple.finder AppleShowAllFiles -boolean true

* Press return
* Now hold ‘alt’ on the keyboard and right click on the Finder icon
* Click on `Relaunch`.

You should find you will now be able to see any hidden files or folders.

One you are done, perform the steps above however, replace the terminal command in step 4 with:
 

>     defaults write com.apple.finder AppleShowAllFiles -boolean false

### Source
[Zendesk - Show hidden files Mac OS X Yosemite - https://bimotech.zendesk.com/hc/en-gb/articles/222133288-Show-hidden-files-Mac-OS-X-Yosemite](https://bimotech.zendesk.com/hc/en-gb/articles/222133288-Show-hidden-files-Mac-OS-X-Yosemite)

[Archive link - https://archive.ph/vsIzx](https://archive.ph/vsIzx)