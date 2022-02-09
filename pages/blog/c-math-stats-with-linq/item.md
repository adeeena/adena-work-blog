---
title: 'C# - Math statistics with Linq'
date: '21:20 23-07-2020'
taxonomy:
    category:
        - 'c#'
        - 'useful extensions'
        - coding
        - linq
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

I have a collection of person objects (IEnumerable) and each person has an age property.

I want to generate stats on the collection such as Max, Min, Average, Median, etc on this age property.

What is the most elegant way of doing this using LINQ?

===

### Max
>     var max = persons.Max(p => p.age);

### Min
>     var min = persons.Min(p => p.age);

### Average
>     var average = persons.Average(p => p.age);

### Median
>     int count = persons.Count();
>     var orderedPersons = persons.OrderBy(p => p.age);
>     float median = orderedPersons.ElementAt(count/2).age + orderedPersons.ElementAt((count-1)/2).age;
>     median /= 2;

### Extension method

>     public class CollectionExtensions
>     {
>     	public double Median(this IList<int> items)
>     	{
>     		int count = items.Count();
>     	    var ordered = items.OrderBy(p => p.age);
>          	double median = items.ElementAt(count/2).age + items.ElementAt((count-1)/2).age;
>     	    return median /= 2;
>     	}
>     }


### Mirror from
[StackOverflow - Math stats with Linq - https://stackoverflow.com/questions/4134366/math-stats-with-linq/10738416#10738416](https://stackoverflow.com/questions/4134366/math-stats-with-linq/10738416#10738416)

