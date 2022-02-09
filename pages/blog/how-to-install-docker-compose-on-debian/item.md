---
title: 'How to run Docker and Docker Compose on Debian 10'
media_order: 'Annotation 2020-07-21 141335.png'
taxonomy:
    category:
        - docker
        - docker-compose
        - debian
        - 'debian 10'
    tag:
        - Linux
        - Docker
        - Devops
        - Debian
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

>     sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
>     curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
>     sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
>     sudo apt-get update
>     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose
>     sudo usermod -aG docker your-user
>     sudo reboot

.. after reboot ..

>     docker un hello-world

![](Annotation%202020-07-21%20141335.png)
     
### Mirror from
[Docker - Install Docker Engine on Debian - https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/)