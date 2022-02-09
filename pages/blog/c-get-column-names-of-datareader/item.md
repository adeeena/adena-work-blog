---
title: 'C# - Can you get the column names from a SqlDataReader?'
taxonomy:
    category:
        - 'c#'
        - coding
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

After connecting to the database, can I get the name of all the columns that were returned in my SqlDataReader?

===

### Solution

>     var reader = cmd.ExecuteReader();
>     
>     var columns = new List<string>();
>     
>     for(int i=0;i<reader.FieldCount;i++)
>     {
>        columns.Add(reader.GetName(i));
>     }

or

>     var columns = Enumerable.Range(0, reader.FieldCount)
>          .Select(reader.GetName).ToList();


### Mirror from
[StackOverflow - Can you get the column names from a SqlDataReader? - https://stackoverflow.com/questions/681653/can-you-get-the-column-names-from-a-sqldatareader](https://stackoverflow.com/questions/681653/can-you-get-the-column-names-from-a-sqldatareader)