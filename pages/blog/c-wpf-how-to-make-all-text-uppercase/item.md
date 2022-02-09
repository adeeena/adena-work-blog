---
title: 'C# - WPF/XAML - How to make all text upper case in TextBlock?'
taxonomy:
    category:
        - 'c#'
        - coding
        - wpf
        - xaml
    tag:
        - Programming
        - 'C#'
        - WPF
        - XAML
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I want all characters in a TextBlock to be displayed in uppercase

>     <TextBlock Name="tbAbc"
>            FontSize="12"
>            TextAlignment="Center"
>            Text="Channel Name"
>            Foreground="{DynamicResource {x:Static
>               r:RibbonSkinResources.RibbonGroupLabelFontColorBrushKey}}" />

The strings are taken through Binding. I don't want to make the strings uppercase in the dictionary itself.

===

### Solution

Implement a custom converter.

>     using System.Globalization;
>     using System.Windows.Data;
>     // ...
>     public class StringToUpperConverter : IValueConverter
>     {
>         public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
>         {
>             if (value != null && value is string )
>             {
>                 return ((string)value).ToUpper();
>             }
>     
>             return value;
>         }
>     
>         public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
>         {
>             return null;
>         }
>     }

Then include that in your XAML as a resource:

>     <local:StringToUpperConverter  x:Key="StringToUpperConverter"/>

And add it to your binding:

>     Converter={StaticResource StringToUpperConverter}

### Mirror from
[StackOverflow - WPF/XAML: How to make all text upper case in TextBlock? - https://stackoverflow.com/questions/24956046/wpf-xaml-how-to-make-all-text-upper-case-in-textblock/24956232#24956232](https://stackoverflow.com/questions/24956046/wpf-xaml-how-to-make-all-text-upper-case-in-textblock/24956232#24956232)