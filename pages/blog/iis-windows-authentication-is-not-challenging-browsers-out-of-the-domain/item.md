---
title: 'IIS - Windows authentication is not challenging browsers out of the domain'
media_order: kaylin-pacheco-5ToyvEJIny8-unsplash.jpg
date: '20:08 13-03-2021'
taxonomy:
    category:
        - IIS
        - Devops
    tag:
        - IIS
        - 'Windos Authentication'
hide_git_sync_repo_link: false
hero_classes: text-light
hero_image: kaylin-pacheco-5ToyvEJIny8-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I have a web application that allows anonymous access and windows authentication.

Basically, the app try to auto-login users by windows authentication if the windows user is in a user list in the application, otherwise it shows the login screen. If no credential is provided, the app then shows a login screen where the user could login using a application internal user.

===

As far as I know, Windows Authentication is a challenging protocol, so I have to return an Unauthorized in order to force the browser to send the credentials.

It works great when are machines inside my domain.

Machine in my domain which the user is in the list:

browser issue request with no credentials.
web app custom authorize filter reject the connection (challenge).
browser reissue the request with the windows credentials.
web app authenticates that windows user and allow it get it.
Machine in my domain which the user is not in the allowed list:

browser issue request with no credentials.
web app custom authorize filter reject the connection (challenge).
browser reissue the request with the windows credentials.
web app fails to authenticate the request and redirect to the login screen.
Now comes the problem.

Machine out of my domain which the user is not in the allowed list:

browser issue request with no credentials.
web app custom authorize filter reject the connection (challenge).
browser shows a windows login screen, and it doesn't reissue the request.
Why the request is not reissued when the server is returning unauthorized?

If I close the auth dialog, and refresh the browser, then the app redirects to the login page.

### Reply

Well, if the computer is outside the server domain, the browser won't try to send the credentials (as it would be probably pointless in terms of Windows authentication) and it will prompt for credentials after the first HTTP 401.

The solution is enable Anonymous authentication and Windows authentication together, and let the LogOn page be accessed anonymously. In the LogOn page place the login controls, and a button named for example "Windows Login" that navigates to an action method named "WindowsAuth". In that action method, return `HttpUnauthorizedResult` if not `Request.LogonUserIdentity.IsAuthenticated`, and redirects to Home otherwise, that will force the browser to send the Windows credentials.

If a browser outside of the domain tries to do a "Windows Login", it will get a nice "HTTP 401 Unauthorized" page, so he can hit back and perform login in the traditional way.

### Source
[StackOverflow](https://stackoverflow.com/questions/7310636/windows-authentication-is-not-challenging-browsers-out-of-the-domain)
 
 
[Archive - https://archive.ph/VvunM](https://archive.ph/VvunM)