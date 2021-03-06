---
title: 'C# - Predicate Builder - Dynamically Composing Expression Predicates'
media_order: xps-g2E2NQ5SWSU-unsplash.jpg
date: '18:06 13-03-2021'
taxonomy:
    category:
        - 'c#'
        - predicate
        - 'predicate builder'
    tag:
        - 'C#'
hide_git_sync_repo_link: false
visible: true
hero_classes: text-dark
hero_image: xps-g2E2NQ5SWSU-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

===

Suppose you want to write a LINQ to SQL or Entity Framework query that implements a keyword-style search. In other words, a query that returns rows whose description contains some or all of a given set of keywords.

We can proceed as follows:

>     IQueryable<Product> SearchProducts (params string[] keywords)
>     {
>       IQueryable<Product> query = dataContext.Products;
>     
>       foreach (string keyword in keywords)
>         query = query.Where (p => p.Description.Contains (keyword));
>     
>       return query;
>     }

So far, so good. But this only handles the case where you want to match all of the specified keywords. Suppose instead, we wanted products whose description contains any of the supplied keywords. Our previous approach of chaining Where operators is completely useless! We could instead chain Union operators, but this would be inefficient. The ideal approach is to dynamically construct a lambda expression tree that performs an or-based predicate.

Of all the things that will drive you to manually constructing expression trees, the need for dynamic predicates is the most common in a typical business application. Fortunately, it’s possible to write a set of simple and reusable extension methods that radically simplify this task. This is the role of our `PredicateBuilder` class.

### Using PredicateBuilder

Here's how to solve the preceding example with `PredicateBuilder`:

>     IQueryable<Product> SearchProducts (params string[] keywords)
>     {
>       var predicate = PredicateBuilder.False<Product>();
>     
>       foreach (string keyword in keywords)
>         predicate = predicate.Or (p => p.Description.Contains (keyword));
>     
>       return dataContext.Products.Where (predicate);
>     }

If querying with Entity Framework, change the last line to this:

>     return objectContext.Products.AsExpandable().Where (predicate);

The `AsExpandable` method is part of LINQKit (see below).

The easiest way to experiment with `PredicateBuilder` is with LINQPad. LINQPad lets you instantly test LINQ queries against a database or local collection and has direct support for `PredicateBuilder` (press F4 and check 'Include PredicateBuilder').

### PredicateBuilder Source Code

Here's the complete source:

>     using System;
>     using System.Linq;
>     using System.Linq.Expressions;
>     using System.Collections.Generic;
>     
>     public static class PredicateBuilder
>     {
>       public static Expression<Func<T, bool>> True<T> ()  { return f => true;  }
>       public static Expression<Func<T, bool>> False<T> () { return f => false; }
>      
>       public static Expression<Func<T, bool>> Or<T> (this Expression<Func<T, bool>> expr1,
>                                                           Expression<Func<T, bool>> expr2)
>       {
>         var invokedExpr = Expression.Invoke (expr2, expr1.Parameters.Cast<Expression> ());
>         return Expression.Lambda<Func<T, bool>>
>               (Expression.OrElse (expr1.Body, invokedExpr), expr1.Parameters);
>       }
>      
>       public static Expression<Func<T, bool>> And<T> (this Expression<Func<T, bool>> expr1,
>                                                            Expression<Func<T, bool>> expr2)
>       {
>         var invokedExpr = Expression.Invoke (expr2, expr1.Parameters.Cast<Expression> ());
>         return Expression.Lambda<Func<T, bool>>
>               (Expression.AndAlso (expr1.Body, invokedExpr), expr1.Parameters);
>       }
>     }

`PredicateBuilder` is also shipped as part of LINQKit, a productivity kit for LINQ to SQL and Entity Framework.

If you're using LINQ to SQL, you can use the `PredicateBuilder` source code on its own.

If you're using Entity Framework, you'll need the complete LINQKit - for the AsExpandable functionality. You can either reference LINQKit.dll or copy LINQKit's source code into your application.

### How it Works

The `True` and `False` methods do nothing special: they are simply convenient shortcuts for creating an `Expression<Func<T,bool>>` that initially evaluates to true or false. So the following:

>     var predicate = PredicateBuilder.True <Product> ();

is just a shortcut for this:

>     Expression<Func<Product, bool>> predicate = c => true;

When you’re building a predicate by repeatedly stacking and/or conditions, it’s useful to have a starting point of either true or false (respectively). Our `SearchProducts` method still works if no keywords are supplied.

The interesting work takes place inside the `And` and `Or` methods. We start by invoking the second expression with the first expression’s parameters. An Invoke expression calls another lambda expression using the given expressions as arguments. We can create the conditional expression from the body of the first expression and the invoked version of the second. The final step is to wrap this in a new lambda expression.

Entity Framework's query processing pipeline cannot handle invocation expressions, which is why you need to call `AsExpandable` on the first object in the query. By calling `AsExpandable`, you activate LINQKit's expression visitor class which substitutes invocation expressions with simpler constructs that Entity Framework can understand.

### More Examples

A useful pattern in writing a data access layer is to create a reusable predicate library. Your queries, then, consist largely of select and orderby clauses, the filtering logic farmed out to your library. Here's a simple example:

>     public partial class Product
>     {
>       public static Expression<Func<Product, bool>> IsSelling()
>       {
>         return p => !p.Discontinued && p.LastSale > DateTime.Now.AddDays (-30);
>       }
>     }

We can extend this by adding a method that uses `PredicateBuilder`:

>     public partial class Product
>     {
>       public static Expression<Func<Product, bool>> ContainsInDescription (
>                                                     params string[] keywords)
>       {
>         var predicate = PredicateBuilder.False<Product>();
>         foreach (string keyword in keywords)
>           predicate = predicate.Or (p => p.Description.Contains (keyword));
>     
>         return predicate;
>       }
>     }

This offers an excellent balance of simplicity and reusability, as well as separating business logic from expression plumbing logic. To retrieve all products whose description contains “BlackBerry” or “iPhone”, along with the Nokias and Ericssons that are selling, you would do this:

>     var newKids  = Product.ContainsInDescription ("BlackBerry", "iPhone");
>     
>     var classics = Product.ContainsInDescription ("Nokia", "Ericsson")
>                           .And (Product.IsSelling());
>     var query =
>       from p in Data.Products.Where (newKids.Or (classics))
>       select p;

The `And` and `Or` methods in boldface resolve to extension methods in `PredicateBuilder`.

An expression predicate can perform the equivalent of an SQL subquery by referencing association properties. So, if Product had a child `EntitySet` called Purchases, we could refine our IsSelling method to return only those products that have sold a minimum number of units as follows:

>     public static Expression<Func<Product, bool>> IsSelling (int minPurchases)
>     {
>       return prod =>
>         !prod.Discontinued &&
>          prod.Purchases.Where (purch => purch.Date > DateTime.Now.AddDays(-30))
>                         .Count() >= minPurchases;
>     }

### Nesting Predicates
Consider the following predicate:

>     p => p.Price > 100 &&
>          p.Price < 1000 &&
>          (p.Description.Contains ("foo") || p.Description.Contains ("far"))

Let's say we wanted to build this dynamically. The question is, how do we deal with the parenthesis around the two expressions in the last line?

The answer is to build the parenthesised expression first, and then consume it in the outer expression as follows:

>     var inner = PredicateBuilder.False<Product>();
>     inner = inner.Or (p => p.Description.Contains ("foo"));
>     inner = inner.Or (p => p.Description.Contains ("far"));
>     
>     var outer = PredicateBuilder.True<Product>();
>     outer = outer.And (p => p.Price > 100);
>     outer = outer.And (p => p.Price < 1000);
>     outer = outer.And (inner);

Notice that with the inner expression, we start with `PredicateBuilder.False` (because we're using the `Or` operator). With the outer expression, however, we start with `PredicateBuilder.True` (because we're using the `And` operator).

### Generic Predicates

Suppose every table in your database has `ValidFrom` and `ValidTo` columns as follows:

>     create table PriceList
>     (
>        ID int not null primary key,
>        Name nvarchar(50) not null,
>        ValidFrom datetime,
>        ValidTo datetime
>     )

To retrieve rows valid as of `DateTime.Now` (the most common case), you'd do this:

>     from p in PriceLists
>     where (p.ValidFrom == null || p.ValidFrom <= DateTime.Now) &&
>           (p.ValidTo   == null || p.ValidTo   >= DateTime.Now)
>     select p.Name

Of course, that logic in bold is likely to be duplicated across multiple queries! No problem: let's define a method in the PriceList class that returns a reusable expression:

>     public static Expression<Func<PriceList, bool>> IsCurrent()
>     {
>        return p => (p.ValidFrom == null || p.ValidFrom <= DateTime.Now) &&
>                    (p.ValidTo   == null || p.ValidTo   >= DateTime.Now);
>     }

OK: our query is now much simpler:

>     var currentPriceLists = db.PriceLists.Where (PriceList.IsCurrent());

And with `PredicateBuilder`'s `And` and `Or` methods, we can easily introduce other conditions:

>     var currentPriceLists = db.PriceLists.Where (
>                               PriceList.IsCurrent().And (p => p.Name.StartsWith ("A")));

But what about all the other tables that also have `ValidFrom` and `ValidTo` columns? We don't want to repeat our `IsCurrent` method for every table! Fortunately, we can generalize our `IsCurrent` method with generics.

The first step is to define an interface:

>     public interface IValidFromTo
>     {
>        DateTime? ValidFrom { get; }
>        DateTime? ValidTo   { get; }
>     }

Now we can define a single generic `IsCurrent` method using that interface as a constraint:

>     public static Expression<Func<TEntity, bool>> IsCurrent<TEntity>()
>        where TEntity : IValidFromTo
>     {
>        return e => (e.ValidFrom == null || e.ValidFrom <= DateTime.Now) &&
>                    (e.ValidTo   == null || e.ValidTo   >= DateTime.Now);
>     }

The final step is to implement this interface in each class that supports `ValidFrom` and `ValidTo`. If you're using Visual Studio or a tool like SqlMetal to generate your entity classes, do this in the non-generated half of the partial classes:

>     public partial class PriceList : IValidFromTo { }
>     public partial class Product   : IValidFromTo { }
    
###Source
[C# in a Nutshell - Predicate Builder - http://www.albahari.com/nutshell/predicatebuilder.aspx](http://www.albahari.com/nutshell/predicatebuilder.aspx)    
    
