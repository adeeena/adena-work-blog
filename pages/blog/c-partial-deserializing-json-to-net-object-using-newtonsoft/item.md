---
title: 'C# - Partial deserializing JSON to .NET object using Newtonsoft'
media_order: alyssa-strohmann-qOIGvGoVNtc-unsplash.jpg
date: '20:30 13-03-2021'
taxonomy:
    category:
        - 'C#'
        - deserialization
        - JSON
    tag:
        - 'C#'
        - JSON
        - Newtonsoft
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: alyssa-strohmann-qOIGvGoVNtc-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

If you just need to get a few items from the JSON object, I would use Json.NET's LINQ to JSON `JObject` class.

===
 For example:

>     JToken token = JObject.Parse(stringFullOfJson);
>     
>     int page = (int)token.SelectToken("page");
>     int totalPages = (int)token.SelectToken("total_pages");

I like this approach because you don't need to fully deserialize the JSON object. This comes in handy with APIs that can sometimes surprise you with missing object properties, like Twitter.

### Source
[StackOverflow - https://stackoverflow.com/questions/4749639/deserializing-json-to-net-object-using-newtonsoft-or-linq-to-json-maybe/4749755#4749755](https://stackoverflow.com/questions/4749639/deserializing-json-to-net-object-using-newtonsoft-or-linq-to-json-maybe/4749755#4749755)

[Archive - https://archive.ph/qJNMo](https://archive.ph/qJNMo)