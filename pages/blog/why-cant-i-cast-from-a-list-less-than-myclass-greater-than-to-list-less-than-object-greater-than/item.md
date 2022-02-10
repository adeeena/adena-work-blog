---
title: 'C# - Why can''t I cast from a List&lt;MyClass&gt; to List&lt;object&gt;?'
media_order: artem-belinski-EhdMZpCuW98-unsplash.jpg
date: '17:55 13-03-2021'
taxonomy:
    category:
        - 'C#'
    tag:
        - 'C#'
hide_git_sync_repo_link: false
visible: true
hero_classes: text-dark
hero_image: artem-belinski-EhdMZpCuW98-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I have a `List` of objects, which are of my type `QuoteHeader` and I want to pass this list as a list of objects to a method which is able to accept a `List<object>`.

My line of code reads...

>     Tools.MyMethod((List<object>)MyListOfQuoteHeaders);

But I get the following error at design time...

>     Cannot convert type 'System.Collections.Generic.List<MyNameSpace.QuoteHeader>' 
>     to 'System.Collections.Generic.List<object>'

Do I need to do anything to my class to allow this? I thought that all classes inherit from object so I can't understand why this wouldn't work?

===

>    IEnumerable<Object> myNewEnumerable = myEnumerable.Cast<Object>();
    
### Source:
    
[Why can't I cast from a List<MyClass> to List<object>? - https://stackoverflow.com/a/5881725/1694775](https://stackoverflow.com/a/5881725/1694775)