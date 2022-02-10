---
title: 'C# - .NET Core: Remove null fields from API JSON response'
media_order: christina-deravedisian-6wHMi9xXPeQ-unsplash.jpg
date: '19:43 13-03-2021'
taxonomy:
    category:
        - 'c#'
        - 'c# useful extensions'
    tag:
        - 'C#'
        - 'C# Useful extensions'
        - 'Text Processing'
hide_git_sync_repo_link: false
hero_classes: text-dark
hero_image: christina-deravedisian-6wHMi9xXPeQ-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
published: true
---

Given the string "ThisStringHasNoSpacesButItDoesHaveCapitals" what is the best way to add spaces before the capital letters. So the end string would be "This String Has No Spaces But It Does Have Capitals"

Here is my attempt with a RegEx:

>     System.Text.RegularExpressions.Regex.Replace(value, "[A-Z]", " $0")

===

### Solution

This function

>     public static string AddSpacesToSentence(this string text, bool preserveAcronyms = false)
>     {
>             if (string.IsNullOrWhiteSpace(text))
>                return string.Empty;
>             StringBuilder newText = new StringBuilder(text.Length * 2);
>             newText.Append(text[0]);
>             for (int i = 1; i < text.Length; i++)
>             {
>                 if (char.IsUpper(text[i]))
>                     if ((text[i - 1] != ' ' && !char.IsUpper(text[i - 1])) ||
>                         (preserveAcronyms && char.IsUpper(text[i - 1]) && 
>                          i < text.Length - 1 && !char.IsUpper(text[i + 1])))
>                         newText.Append(' ');
>                 newText.Append(text[i]);
>             }
>             return newText.ToString();
>     }
     

Will do it 100,000 times in 2,968,750 ticks, the regex will take 25,000,000 ticks (and thats with the regex compiled).

It's better, for a given value of better (i.e. faster) however it's more code to maintain. "Better" is often compromise of competing requirements.

### Source
[StackOverflow - C# - Add spaces before Capital Letters - https://stackoverflow.com/questions/272633/add-spaces-before-capital-letters](https://stackoverflow.com/questions/272633/add-spaces-before-capital-letters)

[Archive link - https://archive.ph/DvhAJ](https://archive.ph/DvhAJ)