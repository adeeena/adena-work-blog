---
title: 'C# - WPF/XAML - How do I bind datatemplate in a resource dictionary?'
date: '13:18 24-07-2020'
taxonomy:
    category:
        - 'c#'
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

I'm trying to bind my elements in a datatemplate that is define in dictionary. Let's make it simple.

I have a simple class

>     public class A { public string Data { get; set; } }

I have a simple view that contains a `ListBox`, with `ItemSources` is a list of class `A`:

>     <ListBox ItemsSource="{Binding AList}">

The point is, when I define Itemplate in view directly, bind works:

>     <ListBox.ItemTemplate>
>        <DataTemplate >
>           <TextBlock Text="{Binding Data}" />
>           <Rectangle Fill="Red" Height="10" Width="10"/>
>        </DataTemplate>
>     </ListBox.ItemTemplate>

This works great.

But when I define this `ItemTemplate` in ResourceDictionary, binding doesn't work?

===

### Solution

Create a new `Dictionary1.xaml`

>     <ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
>                         xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
>         <DataTemplate x:Key="dataTemplate">
>             <StackPanel>
>                 <TextBlock Text="{Binding Data}" />
>                 <Rectangle Fill="Red" Height="10" Width="10"/>
>             </StackPanel>
>         </DataTemplate>
>     </ResourceDictionary>

In `MainWindow.xaml` refer it

>     <Window.Resources>
>         <ResourceDictionary Source="Dictionary1.xaml" />
>     </Window.Resources>
>     <ListBox Name="lst" ItemTemplate="{StaticResource dataTemplate}"></ListBox>

`MainWindow.cs`:

>      public partial class MainWindow : Window
>      {
>          public MainWindow()
>          {
>              InitializeComponent();
>          }
>      
>          private void MainWindow_OnLoaded(object sender, RoutedEventArgs e)
>          {
>              var observable = new ObservableCollection<Test>();
>              observable.Add(new Test("A"));
>              observable.Add(new Test("B"));
>              observable.Add(new Test("C"));
>              this.lst.ItemsSource = observable;
>          }
>      }
>      
>      public class Test
>      {
>          public Test(string dateTime)
>          {
>              this.Data = dateTime;
>          }
>      
>          public string Data { get; set; }
>      }
    
### Mirror from
[StackOverflow - How do I bind datatemplate in a resource dictionary - https://stackoverflow.com/questions/29776976/how-do-i-bind-datatemplate-in-a-resource-dictionary](https://stackoverflow.com/questions/29776976/how-do-i-bind-datatemplate-in-a-resource-dictionary)