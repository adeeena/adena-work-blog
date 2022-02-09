---
title: 'Javascript - Resetting a setTimeout'
date: '21:01 19-07-2020'
taxonomy:
    category:
        - 'resetting setTimeout'
        - setTimeout
        - 'vanilla js'
    tag:
        - Programming
        - Javascript
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I have the following:

>     window.setTimeout(function() {
>         window.location.href = 'file.php';
>     }, 115000);

How can I, via a .click function, reset the counter midway through the countdown?

===

You can store a reference to that timeout, and then call clearTimeout on that reference.

>     // in the example above, assign the result
>     var timeoutHandle = window.setTimeout(...);
>     
>     // in your click function, call clearTimeout
>     window.clearTimeout(timeoutHandle);
>     
>     // then call setTimeout again to reset the timer
>     timeoutHandle = window.setTimeout(...);

### Mirror from
[StackOverflow - Resetting a setTimeout - https://stackoverflow.com/questions/1472705/resetting-a-settimeout/1472717#1472717](https://stackoverflow.com/questions/1472705/resetting-a-settimeout/1472717#1472717)