---
title: 'C# - Remove all vowels from a string'
date: '21:20 23-07-2020'
taxonomy:
    category:
        - 'c#'
        - 'useful extensions'
        - coding
    tag:
        - Programming
        - 'C#'
        - 'C# Useful extensions'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I am coding a C# application where the user can specify a name and the program will return the name without the vowels. But how does the program understand that if the name contains A, E, I, O, U, Y, then the letters shall be removed.

===

Just remove all the vowels (same for upper case) and assign it to the name again:

>     string vowels = "aeiouy";
>     string name = "Some Name with vowels";
>     name = new string(name.Where(c => !vowels.Contains(c)).ToArray());

### Mirror from
[StackOverflow - Remove all vowels from a name - stackoverflow.com/questions/5676733/remove-all-vowels-from-a-name/](stackoverflow.com/questions/5676733/remove-all-vowels-from-a-name/)

