---
title: 'C# - Discovering derived types using reflection'
media_order: lerone-pieters-Vf7Y6XrcjoU-unsplash.jpg
date: '20:13 13-03-2021'
taxonomy:
    category:
        - 'c#'
        - 'useful extensions'
    tag:
        - 'C#'
        - 'C# Useful extensions'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: lerone-pieters-Vf7Y6XrcjoU-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Using reflection, is it possible to discover all types that derive from a given type?

Presumably the scope would be limited to within a single assembly.

===
### Solution

>     public static List<Type> FindAllDerivedTypes<T>()
>         {
>             return FindAllDerivedTypes<T>(Assembly.GetAssembly(typeof(T)));
>         }
>     
>         public static List<Type> FindAllDerivedTypes<T>(Assembly assembly)
>         {
>             var derivedType = typeof(T);
>             return assembly
>                 .GetTypes()
>                 .Where(t =>
>                     t != derivedType &&
>                     derivedType.IsAssignableFrom(t)
>                     ).ToList();
>         } 

used like:

>     var output = FindAllDerivedTypes<System.IO.Stream>();
>                 foreach (var type in output)
>                 {
>                     Console.WriteLine(type.Name);
>                 }

outputs:

>     NullStream
>     SyncStream
>     __ConsoleStream
>     BufferedStream
>     FileStream
>     MemoryStream
>     UnmanagedMemoryStream
>     PinnedBufferMemoryStream
>     UnmanagedMemoryStreamWrapper
>     IsolatedStorageFileStream
>     CryptoStream
>     TailStream

### Source
[StackOverflow - Discovering derived types using reflection - https://stackoverflow.com/questions/2362580/discovering-derived-types-using-reflection](https://stackoverflow.com/questions/2362580/discovering-derived-types-using-reflection)
    
[Archive](https://archive.ph/WnT7K)