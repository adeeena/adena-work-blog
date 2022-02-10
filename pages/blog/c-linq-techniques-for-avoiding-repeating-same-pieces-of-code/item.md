---
title: 'C# - Linq - Techniques for avoiding repeating same pieces of code'
media_order: saad-chaudhry-pr08I2yvohs-unsplash.jpg
date: '20:19 13-03-2021'
taxonomy:
    category:
        - 'c#'
        - 'useful extensions'
    tag:
        - 'C# Useful extensions'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: saad-chaudhry-pr08I2yvohs-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I am writing a piece of code for C# Web Api, letting the clients to pass a column name and sort direction as parameter. Although there are, like, 30-ish properties, so the following code (despite it works) gets ugly after a while.

What are my options to avoid repeating this seemingly same pieces of code?

===

>     if (column == nameof(MyModel.ColumnA).ToLower())
>     {
>         if (parameters.IsOrderByAsc)
>         {
>             return queryResult.OrderBy(q => q.ColumnA);
>         }
>     
>         return queryResult.OrderByDescending(q => q.ColumnA);
>     }
>     
>     if (column == nameof(MyModel.ColumnB).ToLower())
>     {
>         if (parameters.IsOrderByAsc)
>         {
>             return queryResult.OrderBy(q => q.ColumnB);
>         }
>     
>         return queryResult.OrderByDescending(q => q.ColumnB);
>     }
>     
>     if (column == nameof(MyModel.ColumnC).ToLower())
>     {
>         if (parameters.IsOrderByAsc)
>         {
>              return queryResult.OrderBy(q => q.ColumnC);
>         }
>     
>         return queryResult.OrderByDescending(q => q.ColumnC);
>     }
>     
>     ....

### Solution
If you don't want to pull in `DynamicLinq` its not a lot of Expression tree work to get exactly what you are looking for. In the constructor getting the methods we need to call for the `Queryable`, I'm assuming this is `IQueryable` and not `IEnumerable`. If `IEnumerable` just swap all the `Queryable` for `Enumerable`. For the `ToLower` matching of the property using the `BindingFlag.IgnoreCase`. Also threw in `ThenBy` as a bonus as wasn't a lot of work.

>     public static class QueryableExtensions
>     {
>         private static readonly MethodInfo _orderBy;
>         private static readonly MethodInfo _orderByDescending;
>         private static readonly MethodInfo _thenBy;
>         private static readonly MethodInfo _thenByDescending;
>     
>         static QueryableExtensions()
>         {
>             Func<IQueryable<object>, Expression<Func<object, object>>, IOrderedQueryable<object>> orderBy = Queryable.OrderBy;
>             _orderBy = orderBy.Method.GetGenericMethodDefinition();
>             orderBy = Queryable.OrderByDescending;
>             _orderByDescending = orderBy.Method.GetGenericMethodDefinition();
>             Func<IOrderedQueryable<object>, Expression<Func<object, object>>, IOrderedQueryable<object>> thenBy = Queryable.ThenBy;
>             _thenBy = thenBy.Method.GetGenericMethodDefinition();
>             thenBy = Queryable.ThenByDescending;
>             _thenByDescending = thenBy.Method.GetGenericMethodDefinition();
>         }

>         public static IOrderedQueryable<TSource> OrderBy<TSource>(this IQueryable<TSource> source, string propertyName, bool orderByAscending = true)
>         {
>             return CreateExpression(source, propertyName, orderByAscending ? _orderBy : _orderByDescending);
>         }
>     
>         public static IOrderedQueryable<TSource> ThenBy<TSource>(this IOrderedQueryable<TSource> source, string propertyName, bool orderByAscending = true)
>        {
>             return CreateExpression(source, propertyName, orderByAscending ? _thenBy : _thenByDescending);
>         }
>         
>         private static IOrderedQueryable<TSource> CreateExpression<TSource>(IQueryable<TSource> source, string propertyName, MethodInfo method)
>         {
>             var propInfo = typeof(TSource).GetProperty(propertyName, BindingFlags.IgnoreCase | BindingFlags.Public | BindingFlags.Instance) ?? throw new ArgumentException(nameof(propertyName));
>             var methodInfo = method.MakeGenericMethod(typeof(TSource), propInfo.PropertyType);
>             var sourceParam = Expression.Parameter(typeof(TSource), "source");
>             var property = Expression.Property(sourceParam, propInfo);
>             var lambda = Expression.Lambda(typeof(Func<,>).MakeGenericType(typeof(TSource), propInfo.PropertyType), property, sourceParam);
>             var call = Expression.Call(methodInfo, source.Expression, lambda);
>             return (IOrderedQueryable<TSource>)source.Provider.CreateQuery<TSource>(call);
>         }
>     }


Now can just call it like `return queryResult.OrderBy(column, parameters.IsOrderByAsc)`.


### Source
[Code Review - https://codereview.stackexchange.com/questions/256079/c-linq-techniques-for-avoiding-repeating-same-pieces-of-code/256123#256123](https://codereview.stackexchange.com/questions/256079/c-linq-techniques-for-avoiding-repeating-same-pieces-of-code/256123#256123)

[Archive](https://archive.ph/Xc1Iu)

