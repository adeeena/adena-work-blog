---
title: 'Installing PHP 7.1 on Raspbian Stretch (Rasperry Pi 3B)'
media_order: IMG-0154.jpg
published: true
date: '17:40 18-04-2020'
hide_git_sync_repo_link: false
visible: true
hero_classes: text-light
hero_image: IMG-0154.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
content:
    items: '- ''@self.children'''
    limit: '5'
    order:
        by: date
        dir: desc
    pagination: '1'
    url_taxonomy_filters: '1'
bricklayer_layout: '1'
display_post_summary:
    enabled: '0'
---

===

### Requirements
Raspberry Pi 3 running Raspbian Stretch with network properly set up

### Installation Steps
#### Step 1: Update the packages list
The default package repositories that come with Raspbian don’t contain PHP 7.1. The package is available, however, in the next-stable Debian repositories – also known as the “testing” repository, containing all the newest packages that should be included in the next stable release, codenamed **buster**. We’ll need to update the sources list in order to change the main repo from stretch, which is the current-stable, to buster, which is the next-stable / testing repository.

Edit the file /etc/apt/sources.list with your text editor of choice:

> sudo nano /etc/apt/sources.list

Now change the source repo from “stretch” to “buster”. The line should look similar to this:

> deb http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free rpi

Save and close the file.

#### Step 2: Update the System
Since this is a different package repository (with newer versions of software you’re currently using), it’s important to perform a full system update afterwards, due to compatibility. More information can be found here.

Run the following (first it will update the packages list, and then upgrade the system)

> sudo apt-get update && sudo apt-get upgrade

#### Step 3: Remove any previous PHP versions
If you had PHP installed before, which was my case, it’s a good idea to remove any old packages beforehand:

> sudo apt-get remove '^php.*'

#### Step 4: Install PHP 7.1
If you’re using the FPM version like I do (with Nginx for instance), you should install the package php7.1-fpm (and probably php7.1-cli as well), and don’t forget to restart nginx afterwards.

> sudo apt-get install php7.1-fpm php7.1-cli

Otherwise, I’m fairly certain you should just go for the metapackage php7.1:

> sudo apt-get install php7.1

And that’s all.

(Source: [http://www.heidislab.com/tutorials/installing-php-7-1-on-raspbian-stretch-raspberry-pi-zero-w]() )