---
title: 'TypeScript - use correct version of setTimeout (node vs window)'
media_order: sajad-nori-WLBvvulifpk-unsplash.jpg
taxonomy:
    category:
        - typescript
        - setTimeout
    tag:
        - TypeScript
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: sajad-nori-WLBvvulifpk-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I am working on upgrading some old TypeScript code to use the latest compiler version, and I'm having trouble with a call to `setTimeout`. The code expects to call the browser's `setTimeout` function which returns a number:

>      setTimeout(handler: (...args: any[]) => void, timeout: number): number;

However, the compiler is resolving this to the node implementation instead, which returns a `NodeJS.Timer`:

 >     setTimeout(callback: (...args: any[]) => void, ms: number, ...args: any[]): NodeJS.Timer;

This code does not run in node, but the node typings are getting pulled in as a dependency to something else (not sure what).

How can I instruct the compiler to pick the version of setTimeout that I want?

Here is the code in question:

>     let n: number;
>     n = setTimeout(function () { /* snip */  }, 500);

This produces the compiler error:

> TS2322: Type 'Timer' is not assignable to type 'number'.

===

### Solution

>     let timer: ReturnType<typeof setTimeout> = setTimeout(() => { ... });

>     clearTimeout(timer);
    
By using `ReturnType<fn>` you are getting independence from platform. You won't be forced to use neither any nor `window.setTimeout` which will break if you run the code on nodeJS server (eg. server-side rendered page).


### Source
    
[StackOverflow - TypeScript - use correct version of setTimeout (node vs window)](https://stackoverflow.com/questions/45802988/typescript-use-correct-version-of-settimeout-node-vs-window)
