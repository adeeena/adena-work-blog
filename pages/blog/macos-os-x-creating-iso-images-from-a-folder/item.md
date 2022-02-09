---
title: 'macOS - OS X - Creating ISO images from a folder'
media_order: xps-DYAf-8UTFN8-unsplash.jpg
date: '09:29 05-09-2020'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Yet one other thing I love about OSX is that .iso is a first-class citizen. I’m able to mount/burn iso files without installing any other software (yes, I’m looking at you Windows).

===

Recently, I had a group of files and folders that I have been transporting around. I had been looking for a way to group these into an ISO so

* they’d be easier to transport and 
* I could burn them to a CD easier.

What I did not know was that with the hdiutil terminal app, I am also able to create an ISO of a folder in OSX without any additional tools.

The syntax is pretty simple:

>     hdiutil makehybrid -o ~/Desktop/image.iso ~/path/to/folder/to/be/converted -iso -joliet

Mission accomplished.

### Mirror from
[Creating ISO images from a folder in OSX - https://matt.berther.io/2008/12/14/creating-iso-images-from-a-folder-in-osx/](https://matt.berther.io/2008/12/14/creating-iso-images-from-a-folder-in-osx/)

[Mirror - Archive.is - https://archive.is/LlTBX](https://archive.is/LlTBX)