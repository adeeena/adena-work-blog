---
title: 'Angular 2+ - How to route?'
media_order: jack-delulio-y8vqhMsKLuA-unsplash.jpg
date: '19:35 13-03-2021'
taxonomy:
    category:
        - angular
        - routing
    tag:
        - 'Angular 2+'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: jack-delulio-y8vqhMsKLuA-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I have an angular 2+ project and when I run it from `localhost:4200/route-a`, it works fine and when I refresh the browser, all works well as expected. However, when I build it with `ng build` and run it from a web server, navigating to `localhost/route-a` returns a 404. Here is my code for routing:
 
>      imports: [BrowserModule, HttpModule, FormsModule, RouterModule.forRoot([
>         { path: 'home', component: HomeComponent },
>         { path: 'route-a', component: RouteAComponent },
>         { path: '', redirectTo: '/home', pathMatch: 'full' }
>       ])]

===

### Solution

In the import of the `RouterModule` where there is something like:

>     RouterModule.forRoot( routes )

You can add the `useHash` this way:

>     RouterModule.forRoot( routes, { useHash: true } )

then rebuild your project with the production flag and the URLs now will be like:

>     http://yourserver/#/page1/

this way, thanks to the hash, the app will work without any problems and the only thing needed is setting the `useHash` on the `RouterModule` and rebuilding your app.

### Source
[StackOverflow - How to route in angular 4 - https://stackoverflow.com/questions/44819308/how-to-route-in-angular-4](https://stackoverflow.com/questions/44819308/how-to-route-in-angular-4)

[Archive.ph link - https://archive.ph/wip/3LPBT](https://archive.ph/3LPBT)