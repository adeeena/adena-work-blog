---
title: 'React Router - Use of ''exact'' prop'
media_order: dom-fou-QZfmPAOnt2Y-unsplash.jpg
date: '20:06 13-03-2021'
taxonomy:
    category:
        - React
        - 'React Router'
        - JavaScript
    tag:
        - React
        - 'React Router'
        - ReactJS
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: dom-fou-QZfmPAOnt2Y-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---


===

You all are aware of how to use react router for loading component for respective routes defined. In this article we will look at the newly introduced keyword ‘exact’ in v4 react router.

Lets take an example, below is how we have a router defined for our app. Consider you have three routes for App, Profile and Dashboard.

>     import React from 'react;
>     import { BrowserRouter, Route } from 'react-router-dom';
>     const App = () => {
>       return (<div>App</div>)
>     }
>     const Dashboard = () => {
>       return (<div>Dashboard</div>)
>     }
>     const Profile = () => {
>       return (<div>Profile</div>)
>     }
>     const Router = () => {
>       return (<BrowserRouter>
>           <Route path='/' component={App}></Route>
>           <Route path='/dashboard' component={Dashboard}></Route>
>           <Route path='/dashboard/profile' component={Profile}></Route>     
>         </BrowserRouter>
>       )
>     }


If you try to visit the routes you will find respective results as below

‘/’ — Will show you ‘App’ on the browser

‘/dashboard’ — Will show you ‘App Dashboard’ on the browser

‘/dashboard/profile’ — Will show you ‘App Dashboard Profile’ on the browser

See the above behaviour when you load ‘/’ route you can see only App is being rendered on the browser but when you load /dashboard then you see both App and Dashboard is loaded on the browser.

So why is it behaving this way ?

So when we try to load `/dashboard` this route patter is matching both `/` and `/dashboard` so it renders both the components. Will be same for `/dashboard/profile` as it matches all the three routes.

So what if you want to display only profile and not dashboard when you visit `/dashboard/profile`. Just add exact keyword on dashboard route and you could only see App and Profile components rendered and Dashboard component is ignored in this path.

>     <Route path='/dashboard' exact component={Dashboard}></Route>

What if you add exact on App route also then try to open `/dashboard/profile` then you will only see Profile component being rendered.

So exact keyword is used only when you want to render a component when there is an exact match of route.

### Source
[Medium - Use of ‘exact’ prop for Route / in react-router v4 - https://medium.com/@sampath.katari/use-of-exact-prop-for-route-in-react-router-v4-fdbe20e8925d](https://medium.com/@sampath.katari/use-of-exact-prop-for-route-in-react-router-v4-fdbe20e8925d)

[Archive link - https://archive.ph/2w9Rn](https://archive.ph/2w9Rn)