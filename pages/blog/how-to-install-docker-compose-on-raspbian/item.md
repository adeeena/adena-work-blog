---
title: 'How to run Docker and Docker Compose on Raspbian / Raspberry Pi'
media_order: DIzsA.png
taxonomy:
    category:
        - docker
        - docker-compose
        - raspbian
        - 'raspberry pi'
    tag:
        - Docker
        - Devops
        - 'Raspberry Pi'
        - Raspbian
        - Linux
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

===

### Installing Docker

>     curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh
>     sudo groupadd docker
>     sudo gpasswd -a pi docker
>     docker run hello-world
     
### Installing Docker Compose

>     sudo apt-get update
>     sudo apt-get install -y python python-pip
>     sudo pip install docker-compose
    
### Mirror from
[HOW TO RUN DOCKER AND DOCKER-COMPOSE ON RASPBIAN - https://manre-universe.net/how-to-run-docker-and-docker-compose-on-raspbian/](https://manre-universe.net/how-to-run-docker-and-docker-compose-on-raspbian/)