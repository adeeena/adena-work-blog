---
title: 'C# - .NET Core: Remove null fields from API JSON response'
media_order: clark-van-der-beken-lNCJD6NXc2U-unsplash.jpg
taxonomy:
    category:
        - 'c#'
        - api
        - JSON
    tag:
        - 'C#'
        - '.NET Core'
hide_git_sync_repo_link: false
hero_classes: text-dark
hero_image: clark-van-der-beken-lNCJD6NXc2U-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
published: true
---

On a global level in .NET Core (all API responses), how can I configure `Startup.cs` so that null fields are removed/ignored in JSON responses?

Using `Newtonsoft.Json`, you can apply the following attribute to a property, but I'd like to avoid having to add it to every single one:

>     [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
>     public string FieldName { get; set; }
>     [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
>     public string OtherName { get; set; }

===

### Solution

#### .NET Core 1.0

In `Startup.cs`, you can attach `JsonOption`s to the service collection and set various configurations, including removing null values, there:

>     public void ConfigureServices(IServiceCollection services)
>     {
>          services.AddMvc()
>                  .AddJsonOptions(options => {
>                     options.SerializerSettings.NullValueHandling = NullValueHandling.Ignore;
>          });
>     }

#### .NET Core 3.1

Instead of this line:

>     options.SerializerSettings.NullValueHandling = NullValueHandling.Ignore;

Use:

>     options.JsonSerializerOptions.IgnoreNullValues = true;

#### .NET 5.0

Instead of both variants above, use:

>     options.JsonSerializerOptions.DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull;

The variant from .NET Core 3.1 still works, but it is marked as `NonBrowsable` (so you never get the IntelliSense hint about this parameter), so it is very likely that it is going to be obsoleted at some point.

### Source
[StackOverflow - .NET Core: Remove null fields from API JSON response - https://stackoverflow.com/questions/44595027/net-core-remove-null-fields-from-api-json-response](https://stackoverflow.com/questions/44595027/net-core-remove-null-fields-from-api-json-response)
