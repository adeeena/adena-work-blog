---
title: 'jQuery - Detecting when user scrolls to bottom of div'
date: '10:54 08-08-2020'
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

I have a div box (called flux) with a variable amount of content inside. This divbox has overflow set to auto.

Now, what I am trying to do, is, when the use scroll to the bottom of this DIV-box, load more content into the page, I know how to do this (load the content) but I don't know how to detect when the user has scrolled to the bottom of the div tag? If I wanted to do it for the entire page, I'd take .scrollTop and subtract that from .height.

But I can't seem to do that here?

I've tried taking .scrollTop from flux, and then wrapping all the content inside a div called inner, but if I take the innerHeight of flux it returns 564px (the div is set to 500 as height) and the height of 'innner' it returns 1064, and the scrolltop, when at the bottom of the div says 564.

What can I do?

===

### Solution

There are some properties/methods you can use:

>     How much has been scrolled
>     $().scrollTop()
>     
>     inner height of the element
>     $().innerHeight()
>     
>     height of the content of the element
>     DOMElement.scrollHeight

So you can take the sum of the first two properties, and when it equals to the last property, you've reached the end:

>     jQuery(function($) {
>         $('#flux').on('scroll', function() {
>             if($(this).scrollTop() + $(this).innerHeight() >= $(this)[0].scrollHeight) {
>                 alert('end reached');
>             }
>         })
>     });

### Mirror from
[StackOverflow - Detecting when user scrolls to bottom of div with jQuery - https://stackoverflow.com/questions/6271237/detecting-when-user-scrolls-to-bottom-of-div-with-jquery/6271466#6271466](https://stackoverflow.com/questions/6271237/detecting-when-user-scrolls-to-bottom-of-div-with-jquery/6271466#6271466)