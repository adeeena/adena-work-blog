---
title: 'CSS - Limit text length to n lines using CSS'
media_order: 'Annotation 2020-07-24 141937.png'
date: '12:14 24-07-2020'
taxonomy:
    category:
        - css
        - text
        - ellipsis
    tag:
        - Programming
        - CSS
        - HTML
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Is it possible to limit a text length to "n" lines using CSS (or cut it when overflows vertically)?

`text-overflow: ellipsis;` only works for 1 line text.

### Original text

Ultrices natoque mus mattis, aliquam, cras in pellentesque
tincidunt elit purus lectus, vel ut aliquet, elementum nunc
nunc rhoncus placerat urna! Sit est sed! Ut penatibus turpis
mus tincidunt! Dapibus sed aenean, magna sagittis, lorem velit

### Wanted output (2 lines)

Ultrices natoque mus mattis, aliquam, cras in pellentesque
tincidunt elit purus lectus, vel ut aliquet, elementum...

===

There's a way to do it using unofficial `line-clamp` syntax, and starting with Firefox 68 it works in all major browsers.


### CSS

>     .text {
>        display:flex;
>        overflow: hidden;
>        text-overflow: ellipsis;
>        -webkit-line-clamp: 2; /* number of lines to show */
>        -webkit-box-orient: vertical;
>     }

### HTML

>     <div class="text">
>     Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam consectetur
>     venenatis blandit. Praesent vehicula, libero non pretium vulputate, lacus arcu
>     facilisis lectus, sed feugiat tellus nulla eu dolor. Nulla porta bibendum lectus
>     quis euismod. Aliquam volutpat ultricies porttitor. Cras risus nisi, accumsan
>     vel cursus ut, sollicitudin vitae dolor. Fusce scelerisque eleifend lectus in bibendum.
>     Suspendisse lacinia egestas felis a volutpat.
>     </div>

### Result
![](Annotation%202020-07-24%20141937.png)

Unless you care about IE users, there is no need to do `line-height` and `max-height` fallbacks.

### Mirror from
[StackOverflow - Limit text length to n lines using CSS - https://stackoverflow.com/questions/3922739/limit-text-length-to-n-lines-using-css](https://stackoverflow.com/questions/3922739/limit-text-length-to-n-lines-using-css)
