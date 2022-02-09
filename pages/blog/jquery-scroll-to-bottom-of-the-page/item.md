---
title: 'jQuery - Scroll to bottom of the page'
taxonomy:
    category:
        - JavaScript
        - jQuery
    tag:
        - JavaScript
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

After my page is done loading. I want jQuery to nicely scroll to the bottom of the page, animating quickly, not a snap/jolt.

===

### Solution

You can just animate to scroll down the page by animating the `scrollTop` property, no plugin required, like this:

>     $(window).load(function() {
>       $("html, body").animate({
>           scrollTop: $(document).height()
>       }, 1000);
>     });

Note the use of `window.onload` (when images are loaded...which occupy height) rather than `document.ready`.

To be technically correct, you need to subtract the window's height, but the above works:

>     $("html, body").animate({
>          scrollTop: $(document).height() - $(window).height()
>     });

To scroll to a particular ID, use its .scrollTop(), like this:

>     $("html, body").animate({
>          scrollTop: $("#myID").scrollTop()
>     }, 1000);

### Mirror from
[StackOverflow - Detecting when user scrolls to bottom of div with jQuery - https://stackoverflow.com/questions/4249353/jquery-scroll-to-bottom-of-the-page](https://stackoverflow.com/questions/4249353/jquery-scroll-to-bottom-of-the-page)