---
title: 'C# LINQ vs. TypeScript vs. Vanilla JavaScript (Chrome <= 30)'
media_order: photo-1597302520641-6c95c8023e1c.jpeg
date: '12:07 08-08-2020'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Nor JavaScript nor TypeScript has no equivalent for the language-integrated-natural-query aspect of LINQ. (hey, isn't that literally the whole acronym?)

===

### Aggregate
>     // C#
>     var leftToRight = users.Aggregate(initialValue, (a, u) => /* ... */);
>     // TypeScript
>     const leftToRight = users.reduce((a, u) => /* ... */, initialValue);
>     const rightToLeft = users.reduceRight((a, u) => /* ... */, initialValue);

### All

>     // C#
>     var allReady = users.All(u => u.IsReady);
>     // TypeScript
>     const allReady = users.every(u => u.isReady);

### Any
>     // C#
>     var isDirty = users.Any(u => u.IsDirty);

>     // TypeScript
>     const isDirty = users.some(u => u.isDirty);

>     // Vanilla JS
>     var isDirty = users.filter(function(u) {
>         return u.isDirty;
>     }).length > 0;

### Append
>     // C#
>     var allUsers = users.Append(oneMoreUser);
>     // TypeScript
>     const allUsers = [ ...users, oneMoreUser ];

### Average
>     // C#
>     var avgAge = users.Average(u => u.Age);
>     // TypeScript
>     if (users.length < 1) {
>       throw new Error('source contains no elements');
>     }
>     const avgAge = users.reduce((a, u) => a + u.age, 0) / users.length;

### Cast

>     // C#
>     var people = users.Cast<Person>();
>     // TypeScript
>     const people = users as Person[];
>     // Note: not semantically the same. The C# version throws an exception
>     // if any of the users can't be cast to type Person.

### Concat

>     // C#
>     var allUsers = users.Concat(moreUsers);
>     // TypeScript
>     const allUsers = [ ...users, ...moreUsers ];

### Contains

>     // C#
>     var hasAdmin = users.Contains(admin);
>     // TypeScript
>     const hasAdmin = users.includes(admin); // Use a polyfill for IE support

### Count

>     // C#
>     var n = users.Count();
>     // TypeScript
>     const n = users.length;

### DefaultIfEmpty

>     // C#
>     var nonEmptyUsers = Enumerable.DefaultIfEmpty(users);
>     // TypeScript
>     const nonEmptyUsers = users.length ? users : [ null ];

### Distinct

>     // C#
>     var uniqueNames = users.Select(u => u.Name).Distinct();
>     // TypeScript
>     const uniqueNames = Object.keys(
>       users.map(u => u.name).reduce(
>         (un, u) => ({ ...un, n }),
>         {}
>       )
>     );

### ElementAt

>     // C#
>     var nth = users.ElementAt(n);
>     // TypeScript
>     if (n < 0 || n > users.length) {
>       throw new Error('Index was out of range');
>     }
>     const nth = users[n];

### ElementAtOrDefault

>     // C#
>     var nth = users.ElementAtOrDefault(n);
>     // TypeScript
>     const nth = users[n];

### Empty

>     // C#
>     var noUsers = IEnumerable.Empty<User>();
>     // TypeScript
>     const noUsers: User[] = [];
>     const noUsers = [] as User[];

### Except

>     // C#
>     var maleUsers = users.Except(femaleUsers);
>     // TypeScript
>     const maleUsers = users.filter(u =>
>       !femaleUsers.includes(u) // Use a polyfill for IE support
>     );

### First

>     // C#
>     var first = users.First();
>     // TypeScript
>     if (users.length < 1) {
>       throw new Error('Sequence contains no elements');
>     }
>     const first = users[0];

### FirstOrDefault

>     // C#
>     var first = users.FirstOrDefault();
>     // TypeScript
>     const first = users[0];
>     List.ForEach
>     // C#
>     users.ToList().ForEach(u => /* ... */);
>     // TypeScript
>     users.forEach(u => /* ... */);

### GroupBy

>     // C#
>     var usersByCountry = users.GroupBy(u => u.Country);
>     // TypeScript
>     const usersByCountry = users.reduce((ubc, u) => ({
>       ...ubc,
>       [u.country]: [ ...(ubc[u.country] || []), u ],
>     }), {});

### Intersect

>     // C#
>     var targetUsers = usersWhoClicked.Intersect(usersBetween25And45);
>     // TypeScript
>     const targetUsers = usersWhoClicked.filter(u =>
>       usersBetween25And45.includes(u) // Use a polyfill for IE support
>     );

### Last

>     // C#
>     var last = users.Last();
>     // TypeScript
>     if (users.length < 1) {
>       throw new Error('Sequence contains no elements');
>     }
>     const last = users[users.length - 1];

### LastOrDefault

>     // C#
>     var last = users.LastOrDefault();
>     // TypeScript
>     const last = users[users.length - 1];

### Max

>     // C#
>     var oldestAge = users.Max(u => u.Age);
>     // TypeScript
>     if (users.length < 1) {
>       throw new Error('source contains no elements');
>     }
>     const oldestAge = users.reduce((oa, u) => Math.max(oa, u.age), 0);

### Min

>     // C#
>     var youngestAge = users.Min(u => u.Age);
>     // TypeScript
>     if (users.length < 1) {
>       throw new Error('source contains no elements');
>     }
>     const youngestAge = users.reduce((ya, u) => Math.min(ya, u.age), Number.MAX_VALUE);

### OfType

>     // C#
>     var bots = users.OfType<Bot>();
>     // TypeScript
>     // No equivalent

### OrderBy / ThenBy

>     // C#
>     var sorted = users.OrderBy(u => u.Age).ThenBy(u => u.Name);
>     // TypeScript
>     const sorted = users.sort((a, b) => {
>       const ageDiff = b.age - a.age;
>       if (ageDiff) return ageDiff;
>       return a.name.localeCompare(b.name); // Use a polyfill for IE support
>     });

### Reverse

>     // C#
>     var backwards = users.Reverse();
>     // TypeScript
>     const backwards = users.reverse();
>     // Caution: users is also reversed!

### Select
>     // C#
>     var names = users.Select(u => u.Name);

>     // TypeScript
>     const names = users.map(u => u.name);

>     // Vanilla JS
>     var names = users.map(function(u) {
>         return u.name;
>     });

### SelectMany

>     // C#
>     var phoneNumbers = users.SelectMany(u => u.PhoneNumbers);
>     // TypeScript
>     const phoneNumbers = users.reduce((pn, u) => [ ...pn, ...u.phoneNumbers ], []);

### Single

>     // C#
>     var user = users.Single();
>     // TypeScript
>     if (users.length > 1) {
>       throw new Error('The input sequence contains more than one element');
>     }
>     else if (!users.length) {
>       throw new Error('The input sequence is empty');
>     }
>     const user = users[0];

### SingleOrDefault

>     // C#
>     var user = users.Single();
>     // TypeScript
>     const user = users[0];

### Skip

>     // C#
>     var otherUsers = users.Skip(n);
>     // TypeScript
>     const otherUsers = users.filter((u, i) => i >= n);

### SkipWhile

>     // C#
>     var otherUsers = users.SkipWhile(predicate);
>     // TypeScript
>     let i = 0;
>     while (i < users.length && predicate(users[i++]));
>     const otherUsers = users.slice(i - 1);

### Sum

>     // C#
>     var totalYears = users.Sum(u => u.Age);
>     // TypeScript
>     if (users.length < 1) {
>       throw new Error('source contains no elements');
>     }
>     const totalYears = users.reduce((ty, u) => ty + u, 0);

### Take

>     // C#
>     var otherUsers = users.Take(n);
>     // TypeScript
>     const otherUsers = users.filter((u, i) => i < n);

### TakeWhile

>     // C#
>     var otherUsers = users.TakeWhile(predicate);
>     // TypeScript
>     let i = 0;
>     while (i < users.length && predicate(users[i++]));
>     const otherUsers = users.slice(0, i - 1);

### Union

>     // C#
>     var allUsers = someUser.Union(otherUsers);
>     // TypeScript
>     const allUsers = otherUsers.reduce((au, u) => 
>       au.includes(u) // Use a polyfill for IE support
>         ? au
>         : [ ...au, u ]
>     }), someUsers));
    
### Where

>     // C#
>     var adults = users.Where(u => u.Age >= 18);

>     // TypeScript
>     const adults = users.filter(u => u.age >= 18);

>     // Vanilla JS
>     var adults = users.filter(function(u) {
>         return u.age >= 18;
>     });

### Zip

>     // C#
>     var matches = buyers.Zip(sellers, (b, s) => new { Buyer = b, Seller = s });
>     // TypeScript
>     const matches = [];
>     for (let i = 0; i < buyers.length && i < sellers.length; i++) {
>       matches.push({
>         buyer: buyers[i],
>         seller: sellers[i],
>       });
>     }
    
### Mirror from
[DecemberSoft - TypeScript vs. C#: LINQ - https://decembersoft.com/posts/typescript-vs-csharp-linq/](https://decembersoft.com/posts/typescript-vs-csharp-linq/)