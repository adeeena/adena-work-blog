---
title: 'C# - WPF/XAML - ListView with horizontal items'
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

I come from WPF and I don't know if it's possible to make a ListView to distribute items horizontally, with all the extras like mouse-wheel scrolling (mouse devices) and swiping (touch devices).

I've tried this, but it doesn't behave like the vertical one. Example: I cannot scroll with the mouse-wheel.

>     <ListView ScrollViewer.VerticalScrollBarVisibility="Disabled" 
>      ScrollViewer.HorizontalScrollBarVisibility="Auto" ItemsSource="{Binding Collection}" >
>         <ListView.ItemsPanel>
>             <ItemsPanelTemplate>
>                 <StackPanel Orientation="Horizontal"></StackPanel>
>             </ItemsPanelTemplate>
>         </ListView.ItemsPanel>
>     </ListView>

===

### Solution

>     <ListView ScrollViewer.VerticalScrollBarVisibility="Disabled"
>     ScrollViewer.HorizontalScrollBarVisibility="Auto"
>         ScrollViewer.HorizontalScrollMode="Enabled"                  
>         ScrollViewer.VerticalScrollMode="Disabled"
>         ItemsSource="{Binding Collection}">
>         <ListView.ItemsPanel>
>             <ItemsPanelTemplate>
>                 <StackPanel Background="Transparent" Orientation="Horizontal" />
>             </ItemsPanelTemplate>
>         </ListView.ItemsPanel>
>     </ListView>

### Mirror from
[StackOverflow - ListView with horizontal items - https://stackoverflow.com/questions/32895171/listview-with-horizontal-items](https://stackoverflow.com/questions/32895171/listview-with-horizontal-items)