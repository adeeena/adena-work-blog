---
title: 'C# - How to remove k__BackingField from json when Deserialize?'
date: '21:20 23-07-2020'
taxonomy:
    category:
        - 'c#'
        - json
        - serialization
        - deserialization
    tag:
        - Programming
        - 'C#'
        - JSON
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I am getting the `k_BackingField` in my returned json after serializing a xml file to a .net c# object.

I've added the `DataContract` and the `DataMember` attribute to the .net c# object but then I get nothing on the json, client end.

>     [XmlRoot("person")]
>     [Serializable]
>     public class LinkedIn
>     {
>         [XmlElement("id")]
>        public string ID { get; set; }
>     
>         [XmlElement("industry")]
>         public string Industry { get; set; }
>     
>         [XmlElement("first-name")]
>         public string FirstName { get; set; }
>     
>         [XmlElement("last-name")]
>         public string LastName { get; set; }
>     }

Example of the returned json:

>     home: Object
>     <FirstName>k__BackingField: "Storefront"
>     <LastName>k__BackingField: "Doors"
    
===
    
### Solution

Remove `[Serializable]` from your class.
    
### Mirror from
[StackOverflow - How to remove k__BackingField from json when Deserialize - https://stackoverflow.com/questions/13022198/how-to-remove-k-backingfield-from-json-when-deserialize/27796357#27796357](https://stackoverflow.com/questions/13022198/how-to-remove-k-backingfield-from-json-when-deserialize/27796357#27796357)