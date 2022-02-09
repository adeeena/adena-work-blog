---
title: 'bash / Git: How to enter command with password for git pull?'
date: '08:25 04-09-2020'
taxonomy:
    category:
        - git
        - bash
    tag:
        - Git
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

I want to do this command in one line:

>     git pull && [my passphrase]

How to do it?

===

I found one way to supply credentials for a https connection on the command line. You just need to specify the complete URL to git pull and include the credentials there:

>     git pull https://username:password@mygithost.com/my/repository

You do not need to have the repository cloned with the credentials before, this means your credentials don't end up in `.git/config`. (But make sure your shell doesn't betray you and stores the command line in a history file.)

### Mirror from
[bash - Git - How to enter command with password for git pull - https://stackoverflow.com/questions/11506124/how-to-enter-command-with-password-for-git-pull](https://stackoverflow.com/questions/11506124/how-to-enter-command-with-password-for-git-pull)