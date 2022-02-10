---
title: 'C# - Find all parent types (both base classes and interfaces)'
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

I want to be able to find all parent types (base classes and interfaces) for a specific type.

E. g. if I have

>     class A : B, C { }
>     class B : D { }
>     interface C : E { }
>     class D { }
>     interface E { }

I want to see that A _is_ B C D and E and Object

Whats the best way to do this? is there a reflection method to do this or do i need to make myself something.

===

### Solution

>     public static bool InheritsFrom(this Type type, Type baseType)
>     {
>         // null does not have base type
>         if (type == null)
>         {
>             return false;
>         }
>     
>         // only interface or object can have null base type
>         if (baseType == null)
>         {
>             return type.IsInterface || type == typeof(object);
>         }
>     
>         // check implemented interfaces
>         if (baseType.IsInterface)
>         {
>             return type.GetInterfaces().Contains(baseType);
>         }
>     
>         // check all base types
>         var currentType = type;
>         while (currentType != null)
>         {
>             if (currentType.BaseType == baseType)
>             {
>                 return true;
>             }
>     
>             currentType = currentType.BaseType;
>         }
>     
>         return false;
>     }

Or to actually get all parent types:

>     public static IEnumerable<Type> GetParentTypes(this Type type)
>     {
>         // is there any base type?
>         if (type == null)
>         {
>             yield break;
>         }
>     
>         // return all implemented or inherited interfaces
>         foreach (var i in type.GetInterfaces())
>         {
>             yield return i;
>         }
>     
>         // return all inherited types
>         var currentBaseType = type.BaseType;
>         while (currentBaseType != null)
>         {
>             yield return currentBaseType;
>             currentBaseType= currentBaseType.BaseType;
>         }
>     }


### Mirror from
[StackOverflow - Find all parent types (both base classes and interfaces)
 - https://stackoverflow.com/questions/8868119/find-all-parent-types-both-base-classes-and-interfaces/18375526#18375526](https://stackoverflow.com/questions/8868119/find-all-parent-types-both-base-classes-and-interfaces/18375526#18375526)