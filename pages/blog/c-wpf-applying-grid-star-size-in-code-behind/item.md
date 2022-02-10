---
title: 'C# - WPF/XAML - Applying Grid Star Size in code behind'
date: '22:17 23-07-2020'
taxonomy:
    category:
        - 'c#'
        - coding
        - wpf
    tag:
        - Programming
        - 'C#'
        - WPF
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

How do I construct this piece of XAML programatically?

>     <Grid Name="gridMarkets">
>         <Grid.RowDefinitions>
>             <RowDefinition Height="10" />
>             <RowDefinition Height="*" MinHeight="16" />
>         </Grid.RowDefinitions>
>         <Grid.ColumnDefinitions>
>             <ColumnDefinition Width="10" />
>             <ColumnDefinition Width="Auto" />
>         </Grid.ColumnDefinitions>
>      </Grid>

Is it any elegant solution for parse and construct controls dynamically?

I was trying to do something:

>     RowDefinition newRow = new RowDefinition();
>     newRow.Height = new GridLength(10);
>     newGrid.RowDefinitions.Add(newRow);

But how do I assign a * sign?

===

### Solution

You can use the `Grid.Star` unit type:

>     newRow.Height = new GridLength(1, GridUnitType.Star);

You can also use the `XamlReader` object to convert XAML strings into UI objects from code-behind, although I usually prefer to manually create objects like how you are creating them.

### Mirror from
[StackOverflow - Applying Grid Star Size in code behind - https://stackoverflow.com/questions/9466688/applying-grid-star-size-in-code-behind/9466711#9466711](https://stackoverflow.com/questions/9466688/applying-grid-star-size-in-code-behind/9466711#9466711)