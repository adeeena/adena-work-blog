---
title: 'C# - Using ping'
date: '13:36 24-07-2020'
taxonomy:
    category:
        - 'c#'
        - 'useful extensions'
        - ping
    tag:
        - 'C#'
        - 'C# Useful extensions'
        - Coding
        - Networking
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

>     using System.Net.NetworkInformation;
>     
>     public static bool PingHost(string nameOrAddress)
>     {
>         bool pingable = false;
>         Ping pinger = null;
>     
>         try
>         {
>             pinger = new Ping();
>             PingReply reply = pinger.Send(nameOrAddress);
>             pingable = reply.Status == IPStatus.Success;
>         }
>         catch (PingException)
>         {
>             // Discard PingExceptions and return false;
>         }
>         finally
>         {
>             if (pinger != null)
>             {
>                 pinger.Dispose();
>             }
>         }
>     
>         return pingable;
>     }
>     

### Mirror from
[StackOverflow - Using ping in c# - https://stackoverflow.com/questions/11800958/using-ping-in-c-sharp/11804416#11804416](https://stackoverflow.com/questions/11800958/using-ping-in-c-sharp/11804416#11804416)