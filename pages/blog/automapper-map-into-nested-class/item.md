---
title: 'Automapper map into nested class'
media_order: cole-wyland-ojkom1zKNQ0-unsplash.jpg
taxonomy:
    tag:
        - 'C#'
        - Automapper
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
hero_image: cole-wyland-ojkom1zKNQ0-unsplash.jpg
---

I have 1 class that I need to map into multiple classes, for eg.

This is the source that I'm mapping from(view model):

>     public class UserBM
>     {
>         public int UserId { get; set; }
>     
>         public string Address { get; set; }
>         public string Address2 { get; set; }
>         public string Address3 { get; set; }
>         public string State { get; set; }
>     
>         public int CountryId { get; set; }
>         public string Country { get; set; }
>     }

This is how the destination class is(domain model):

>     public abstract class User
>     {
>         public int UserId { get; set; }
>     
>         public virtual Location Location { get; set; }
>         public virtual int? LocationId { get; set; }
>     }
>     
>     public class Location
>     {
>         public int LocationId { get; set; }
>     
>         public string Address { get; set; }
>         public string Address2 { get; set; }
>         public string Address3 { get; set; }
>         public string State { get; set; }
>     
>         public virtual int CountryId { get; set; }
>         public virtual Country Country { get; set; }
>     
>     }

This is how my automapper create map currently looks:

>     Mapper.CreateMap<UserBM, User>();


===

### Solution

Define two mappings, both mapping from the same source to different destinations. In the User mapping, map the Location property manually using `Mapper.Map<UserBM, Location>(...)`

>     Mapper.CreateMap<UserBM, Location>();
>     Mapper.CreateMap<UserBM, User>()
>         .ForMember(dest => dest.Location, opt => 
>              opt.MapFrom(src => Mapper.Map<UserBM, Location>(src));

### Mirror from
[Automapper map into nested class](https://stackoverflow.com/questions/7924185/automapper-map-into-nested-class/)



