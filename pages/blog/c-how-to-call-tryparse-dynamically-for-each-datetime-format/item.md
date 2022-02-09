---
title: 'C# - How to call TryParse dynamically for each DateTime format?'
taxonomy:
    category:
        - 'c#'
        - coding
        - 'useful extensions'
    tag:
        - 'C#'
        - DateTime
        - TryParse
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

You could call the `TryParse` method dynamically using Reflection. This way you won't get a time consuming Exception if the conversion fails.

===

>     public static DateTime? TryConvertValue(string stringValue)
>     {
>         DateTime convertedValue = null;
>     
>         if (string.IsNullOrEmpty(stringValue))
>         {
>             return null;
>         }
>         
>         Type[] argTypes = { typeof(string), targetType.MakeByRefType() };
>         var tryParseMethodInfo = typeof(DateTime).GetMethod("TryParse", argTypes);
>         if (tryParseMethodInfo == null)
>         {
>             return null;
>         }
>     
>         object[] args = { stringValue, null };
>         var successfulParse = (bool)tryParseMethodInfo.Invoke(null, args);
>         if (!successfulParse)
>         {
>             return null;
>         }
>     
>         convertedValue = (T)args[1];
>         return null;
>     }
>     

### Mirror from
[StackOverflow - How to call TryParse dynamically - https://stackoverflow.com/questions/11481853/how-to-call-tryparse-dynamically](https://stackoverflow.com/questions/11481853/how-to-call-tryparse-dynamically)

[Archive.is mirror - https://archive.vn/NF662](https://archive.vn/NF662)
