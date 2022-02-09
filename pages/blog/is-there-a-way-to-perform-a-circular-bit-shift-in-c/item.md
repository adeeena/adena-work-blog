---
title: 'Is there a way to perform a circular bit shift in C#?'
date: '21:06 19-07-2020'
taxonomy:
    category:
        - 'c#'
        - 'bit manipulation'
        - 'bit shift'
    tag:
        - Programming
        - 'C#'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I know that the following is true

>     int i = 17; //binary 10001
>     int j = i << 1; //decimal 34, binary 100010

But, if you shift too far, the bits fall off the end. Where this happens is a matter of the size of integer you are working with.

Is there a way to perform a shift so that the bits rotate around to the other side? I'm looking for a single operation, not a for loop.

===

If you know the size of type, you could do something like:

>     uint i = 17;
>     uint j = i << 1 | i >> 31;

... which would perform a circular shift of a 32 bit value.

As a generalization to circular shift left n bits, on a b bit variable:

>     /*some unsigned numeric type*/ input = 17;
>     var result = input  << n | input  >> (b - n);
>     

### Mirror from
[StackOverflow - bit manipulation - is there a way to perform a circular bit shift in C#? - stackoverflow.com/questions/35167/is-there-a-way-to-perform-a-circular-bit-shift-in-c/](stackoverflow.com/questions/35167/is-there-a-way-to-perform-a-circular-bit-shift-in-c/)