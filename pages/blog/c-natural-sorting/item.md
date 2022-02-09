---
title: 'C# - Natural Sorting'
date: '13:25 24-07-2020'
taxonomy:
    category:
        - 'c#'
        - 'c# useful extensions'
    tag:
        - Programming
        - 'C# Useful extensions'
        - 'c#'
        - 'Natural Storting'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

[Jeff Atwood](http://www.codinghorror.com/blog/) posted about [natural sorting](http://www.codinghorror.com/blog/archives/001018.html). This is all about making sure that strings that contain numbers sort numerically. I’m slightly surprised to see that he wants to call it alphabetical sorting. Surely by definition, alphabetical sorting is defined by, well, the alphabet. This is an issue about numbers, not letters.

Anyway, he says he tried and gave up on a succinct C# version. He suggests that it will take 40+ lines of code. I believe that’s misleading, because as far as I can tell, the Python versions are only able to be so succinct because Python already appears to know how to sort an array. Both examples he shows rely on this. In .NET, collections aren’t intrinsically sortable.

===

>     /// <summary>
>     /// Compares two sequences.
>     /// </summary>
>     /// <typeparam name="T">Type of item in the sequences.</typeparam>
>     /// <remarks>
>     /// Compares elements from the two input sequences in turn. If we
>     /// run out of list before finding unequal elements, then the shorter
>     /// list is deemed to be the lesser list.
>     /// </remarks>
>     public class EnumerableComparer<T> : IComparer<IEnumerable<T>>
>     {
>         /// <summary>
>         /// Create a sequence comparer using the default comparer for T.
>         /// </summary>
>         public EnumerableComparer()
>         {
>             comp = Comparer<T>.Default;
>         }
>     
>         /// <summary>
>         /// Create a sequence comparer, using the specified item comparer
>         /// for T.
>         /// </summary>
>         /// <param name="comparer">Comparer for comparing each pair of
>         /// items from the sequences.</param>
>         public EnumerableComparer(IComparer<T> comparer)
>         {
>             comp = comparer;
>         }
>     
>         /// <summary>
>         /// Object used for comparing each element.
>         /// </summary>
>         private IComparer<T> comp;
>     
>     
>         /// <summary>
>         /// Compare two sequences of T.
>         /// </summary>
>         /// <param name="x">First sequence.</param>
>         /// <param name="y">Second sequence.</param>
>         public int Compare(IEnumerable<T> x, IEnumerable<T> y)
>         {
>             using (IEnumerator<T> leftIt = x.GetEnumerator())
>             using (IEnumerator<T> rightIt = y.GetEnumerator())
>             {
>                 while (true)
>                 {
>                     bool left = leftIt.MoveNext();
>                     bool right = rightIt.MoveNext();
>     
>                     if (!(left || right)) return 0;
>     
>                     if (!left) return -1;
>                     if (!right) return 1;
>     
>                     int itemResult = comp.Compare(leftIt.Current, rightIt.Current);
>                     if (itemResult != 0) return itemResult;
>                 }
>             }
>         }
>     }

So yes, I need a lot of code. However, that’s a utility class that is applicable to a wide range of scenarios, not just this one. It’s slightly irritating that it’s not already built into the .NET framework. Heck, maybe it is, and I’ve just been looking in the wrong place.

Given easy way to compare two sequences, a C# 3.0 natural sort becomes roughly as trivial as the Python examples in Jeff’s blog:

>     string[] testItems = { "z24", "z2", "z15", "z1",
>                            "z3", "z20", "z5", "z11",
>                            "z 21", "z22" };
>     
>     Func<string, object> convert = str =>
>     {   
>         try
>         {
>             return int.Parse(str);
>         }
>         catch
>         {
>             return str;
>         }
>     };
>     var sorted = testItems.OrderBy(str =>
>         Regex.Split(str.Replace(" ", ""), "([0-9]+)").Select(convert),
>         new EnumerableComparer<object>());    

If I print out the results using this code:

>      foreach (string s in sorted)
>      {
>          Console.WriteLine(s);
>      }

It prints out the test items in this order:

>      z1
>      z2
>      z3
>      z5
>      z11
>      z15
>      z20
>      z 21
>      z22
>      z24

I.e., ascending numeric order, rather than what you’d get with most string ordering.

### Mirror from
[IanG on Tap - Natural Sorting in C# - http://www.interact-sw.co.uk/iangblog/2007/12/13/natural-sorting](http://www.interact-sw.co.uk/iangblog/2007/12/13/natural-sorting)