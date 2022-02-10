---
title: 'Change SSH port in Debian'
media_order: 'Screen Shot 2020-07-23 at 06.38.13.png,Screen Shot 2020-07-23 at 06.41.04.png'
date: '04:09 23-05-2020'
taxonomy:
    category:
        - SSH
        - Security
        - 'Port changing'
    tag:
        - Linux
        - SSH
        - Security
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Exposing a server to the internet might be pretty scary. You would like to limit the number of exposed services to a minimum. Probably the most powerful service on any Linux machine would be the ssh server. With it you can control the computer and let it perform literally any task. If you want to be able to control your Linux based computer, from any place on the internet you may want to guard yourself against unauthorized access.

===

### Changing the port

Open `/etc/ssh/ssh_config` file, and find the line "Ports". By default, it's commented out (has a `#` in the beginning of the line), meaning it falls back to default port 22.

![](Screen%20Shot%202020-07-23%20at%2006.38.13.png)

Uncomment that line and set another port number (ideally greater than 1024, but lower than 65536). Check if the new port does not conflict with any of your already used service ports. [A list of commonly used services' ports can be found here](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers).

![](Screen%20Shot%202020-07-23%20at%2006.41.04.png)

After saving, restart the ssh service:

>    sudo systemctl restart sshd
    
### Check Before You Disconnect!
Don't disconnect from your terminal yet! Check, with a second terminal, whether the new setup actually works. If it doesn't you'll still have the chance to fix it with your already existing terminal.

If you do lock yourself out you can always still login directly using the console. However that will be rather difficult on a remote machine.