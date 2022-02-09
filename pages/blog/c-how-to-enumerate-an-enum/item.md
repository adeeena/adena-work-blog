---
title: 'C# - How to enumerate an enum?'
date: '21:20 23-07-2020'
taxonomy:
    category:
        - 'c#'
        - coding
        - 'useful extensions'
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

How can you enumerate an enum in C#?

>     public enum Suit
>     {
>         Spades,
>         Hearts,
>         Clubs,
>         Diamonds
>     }

===

>     foreach (Suit suit in (Suit[]) Enum.GetValues(typeof(Suit)))
>     {
>     }

### Extension Method

>     public static class EnumExtensions
>     {
>         /// <summary>
>         /// Gets all items for an enum value.
>         /// </summary>
>         /// <typeparam name="T"></typeparam>
>         /// <param name="value">The value.</param>
>         /// <returns></returns>
>         public static IEnumerable<T> GetAllItems<T>(this Enum value)
>         {
>             foreach (object item in Enum.GetValues(typeof(T)))
>             {
>                 yield return (T)item;
>             }
>         }
>     
>         /// <summary>
>         /// Gets all items for an enum type.
>         /// </summary>
>         /// <typeparam name="T"></typeparam>
>         /// <param name="value">The value.</param>
>         /// <returns></returns>
>         public static IEnumerable<T> GetAllItems<T>() where T : struct
>         {
>             foreach (object item in Enum.GetValues(typeof(T)))
>             {
>                 yield return (T)item;
>             }
>         }
>     
>         /// <summary>
>         /// Gets all combined items from an enum value.
>         /// </summary>
>         /// <typeparam name="T"></typeparam>
>         /// <param name="value">The value.</param>
>         /// <returns></returns>
>         /// <example>
>         /// Displays ValueA and ValueB.
>         /// <code>
>         /// EnumExample dummy = EnumExample.Combi;
>         /// foreach (var item in dummy.GetAllSelectedItems<EnumExample>())
>         /// {
>         ///    Console.WriteLine(item);
>         /// }
>         /// </code>
>         /// </example>
>         public static IEnumerable<T> GetAllSelectedItems<T>(this Enum value)
>         {
>             int valueAsInt = Convert.ToInt32(value, CultureInfo.InvariantCulture);
>     
>             foreach (object item in Enum.GetValues(typeof(T)))
>             {
>                 int itemAsInt = Convert.ToInt32(item, CultureInfo.InvariantCulture);
>     
>                 if (itemAsInt == (valueAsInt & itemAsInt))
>                 {
>                     yield return (T)item;
>                 }
>             }
>         }
>     
>         /// <summary>
>         /// Determines whether the enum value contains a specific value.
>         /// </summary>
>         /// <param name="value">The value.</param>
>         /// <param name="request">The request.</param>
>         /// <returns>
>         ///     <c>true</c> if value contains the specified value; otherwise, <c>false</c>.
>         /// </returns>
>         /// <example>
>         /// <code>
>         /// EnumExample dummy = EnumExample.Combi;
>         /// if (dummy.Contains<EnumExample>(EnumExample.ValueA))
>         /// {
>         ///     Console.WriteLine("dummy contains EnumExample.ValueA");
>         /// }
>         /// </code>
>         /// </example>
>         public static bool Contains<T>(this Enum value, T request)
>         {
>             int valueAsInt = Convert.ToInt32(value, CultureInfo.InvariantCulture);
>             int requestAsInt = Convert.ToInt32(request, CultureInfo.InvariantCulture);
>     
>             if (requestAsInt == (valueAsInt & requestAsInt))
>             {
>                 return true;
>             }
>     
>             return false;
>         }
>     }
>     

### Mirror from
[StackOverflow - How to enumerate an enum - https://stackoverflow.com/questions/105372/how-to-enumerate-an-enum/105402#105402](https://stackoverflow.com/questions/105372/how-to-enumerate-an-enum/105402#105402)