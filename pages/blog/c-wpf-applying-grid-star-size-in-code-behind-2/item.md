---
title: 'C# - WPF/XAML - How to bind an enum object to the background of a button?'
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

I want to bind the background color of a button to an enum. I wonder if there is an enum object that can hold multiple values, for example the state and the color. I want to avoid two enums that could be out of sync. Here are two enums that I would like to integrate with each other.

>     enum StateValue { Player, Wall, Box }
>     enum StateColor { Colors.Red, Colors.Grey, Colors.Brown }

Then I need to create a binding for the XAML button.

>     <Button Content="Player" Background="{Binding Source=...?}" />

Maybe, a dictionary like the following is helpful. But still I do not know how the binding needs to be written.

>     public Dictionary<StateValue, Color> stateValueColor = 
>     new Dictionary<ElementState, Color>()
>     {
>      { StateValue.Player, Colors.Red },
>      { StateValue.Wall, Colors.Grey },
>      { StateValue.Box, Colors.Brown }
>     };

===

### Solution

I would advise you to use a custom converter for that, and only supply the enum from code-behind (or ViewModel).

This would look like this:

>     [ValueConversion(typeof(StateValue), typeof(Color))]
>     public class StateValueColorConverter : IValueConverter
>     {
>         public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
>         {
>             if(!(value is StateValue))
>                 throw new ArgumentException("value not of type StateValue");
>             StateValue sv = (StateValue)value;
>             //sanity checks
>             switch (sv)
>                 {
>                     case StateValue.Player:
>                     return Colors.Red;
>                     //etc
>                 }
>         }
>     
>         public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
>         {
>            Color c = (value as Color);
>            if(c == Colors.Red)
>              return StateValue.Player;
>            //etc
>         }
>     }

And in wpf:

>     <Button 
>       Content="Player" 
>       Background="{Binding Source=StateValue, 
>         Converter={StaticResource stateValueColorConverter}}" />

Of course, if you are using a DI container, you could provide different implementations of the interface with changing colours, just let your view get a converter injected and supply that via binding to the converter property of the binding.

_Note:_ this code was written without compiler, I'm not sure if the xaml is 100% correct, but you should get the idea.


### Mirror from
[StackOverflow - How to bind an enum object to the background of a button? - https://stackoverflow.com/questions/4129270/how-to-bind-an-enum-object-to-the-background-of-a-button/4129307#4129307](https://stackoverflow.com/questions/4129270/how-to-bind-an-enum-object-to-the-background-of-a-button/4129307#4129307)