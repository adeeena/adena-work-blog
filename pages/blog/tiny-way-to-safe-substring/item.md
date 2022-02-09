---
title: 'Tiny way to safe substring'
taxonomy:
    category:
        - 'safe substring'
        - 'useful extensions'
        - 'c# useful extensions'
    tag:
        - Coding
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

===
### Problem:

>     public string ShortDescription
>     {
>          get { return this.Description.Length <= 25 ? this.Description : this.Description.Substring(0, 25) + "..."; }
>     }


I would have liked to just do string.Substring(0, 25) but it throws an exception if the string is less than the length supplied.

### Solution:

>     public static class StringExtensions
>     {
>         public static string SafeSubstring(this string input, int startIndex, int length, string suffix)
>         {
>             if (input.Length >= (startIndex + length))
>             {
>                 if (suffix == null) suffix = string.Empty;
>                 return input.Substring(startIndex, length) + suffix;
>             }
>             else
>             {
>                 if (input.Length > startIndex)
>                 {
>                     return input.Substring(startIndex);
>                 }
>                 else
>                 {
>                     return string.Empty;
>                 }
>             }
>         }
>     }
>     

### Mirror from
[StackOverflow - c# - Tiny way to get the first 25 characters - https://stackoverflow.com/questions/595004/tiny-way-to-get-the-first-25-characters](https://stackoverflow.com/questions/595004/tiny-way-to-get-the-first-25-characters)