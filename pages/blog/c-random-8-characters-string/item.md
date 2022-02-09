---
title: 'C# - How can I generate random alphanumeric strings?'
taxonomy:
    category:
        - 'c#'
        - 'useful extensions'
        - coding
        - .net
        - random
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

How can I generate a random 8 character alphanumeric string in C#?

===

>     private static Random random = new Random();
>     public static string RandomString(int length)
>     {
>         const string chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
>         return new string(Enumerable.Repeat(chars, length)
>           .Select(s => s[random.Next(s.Length)]).ToArray());
>     }

(Note: The use of the `Random` class makes this unsuitable for anything security related, such as creating passwords or tokens. Use the `RNGCryptoServiceProvider` class if you need a strong random number generator.)

### Mirror from
[StackOverflow - C# - How can I generate random alphanumeric strings? - https://stackoverflow.com/questions/1344221/how-can-i-generate-random-alphanumeric-strings/1344242#1344242](https://stackoverflow.com/questions/1344221/how-can-i-generate-random-alphanumeric-strings/1344242#1344242)

