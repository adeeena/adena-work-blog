---
title: 'Cross-Origin PostMessage method fails'
media_order: tasos-mansour-jHMQHLP05RQ-unsplash.jpg
date: '15:25 15-03-2021'
taxonomy:
    tag:
        - http
        - javascript
        - 'cross domain'
hide_git_sync_repo_link: false
visible: true
hero_classes: text-dark
hero_image: tasos-mansour-jHMQHLP05RQ-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I have 2 MVC4 sites in development. SSO (which is HTTPS) and Videos (which is HTTP)
(both local host right now)

Videos loads a page with an IFRAME pointing to a page on SSO. It loads the pages correctly.

===

The SSO page has this javascript:

>     window.onmessage = function (e) {
>         if (e.data == 'hello') {
>             alert('It works!');
>         }
>     };

The Video Page has this code:

>     <iframe frameborder="0" width="100px" height="100px" id="LbpSsoFrame" src="https://localhost:44301/Sso/InFrame"></iframe>
>    
After the page is loaded, I use chrome's console (Chrome V32.0.1700.41 m Aura) and call the following line of code:

>     LbpSsoFrame.contentWindow.postMessage('hello', '*');

I get this error:

>     code: 18
>     message: "Blocked a frame with origin "http://localhost:46086" from accessing a cross-origin frame."
>     name: "SecurityError"
>     stack: "Error: Blocked a frame with origin "http://localhost:46086" from accessing a cross-origin frame.
>     at <anonymous>:2:12
>     at Object.InjectedScript._evaluateOn (<anonymous>:603:39)
>     at Object.InjectedScript._evaluateAndWrap (<anonymous>:562:52)
>     at Object.InjectedScript.evaluate (<anonymous>:481:21)"

I have am up in front of a brick wall, and hoping someone else can knows what I am doing wrong or what else needs to be done. Thanks.
    
### Solution

I should not have been calling on `LbpSsoFrame` as an object.

This is the code that works:

>     document.getElementById('LbpSsoFrame').contentWindow.postMessage('hello', '*');

This is what I had:

>     LbpSsoFrame.contentWindow.postMessage('hello', '*');

With that line fixed, I am getting messages across.

I would have thought that

>     document.getElementById('LbpSsoFrame')

and

>     LbpSsoFrame

were different ways to call the same thing.
    
### Source
[StackOverflow - Cross-Origin PostMessage method fails - https://stackoverflow.com/questions/20804164/cross-origin-postmessage-method-fails](https://stackoverflow.com/questions/20804164/cross-origin-postmessage-method-fails)
[Archive](https://archive.ph/UJ6YN)