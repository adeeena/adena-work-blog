---
title: 'C# - Insert spaces between words on a camel-cased token'
taxonomy:
    category:
        - 'c#'
        - coding
        - 'useful extensions'
        - .net
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

Is there a nice function to to turn something like

FirstName

to this:

First Name?

===

>     Regex.Replace("ThisIsMyCapsDelimitedString", "(\\B[A-Z])", " $1")


### Mirror from
[StackOverflow - C# - Insert spaces between words on a camel-cased token - https://stackoverflow.com/questions/5796383/insert-spaces-between-words-on-a-camel-cased-token/5796427#5796427](https://stackoverflow.com/questions/5796383/insert-spaces-between-words-on-a-camel-cased-token/5796427#5796427)

