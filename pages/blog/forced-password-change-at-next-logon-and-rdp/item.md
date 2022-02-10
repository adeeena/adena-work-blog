---
title: 'Forced password change at next logon and RDP'
media_order: 'clip_image001_thumb.png,image_thumb2.png,image_thumb3.png,image_thumb4.png,clip_image003_thumb.png,image_thumb5.png,image_thumb6.png'
date: '08:30 04-06-2020'
taxonomy:
    category:
        - rdp
        - 'remote desktop'
        - security
    tag:
        - Microsoft
        - 'Password Change'
        - RDP
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

===

If your AD account has the “User must change password at next logon” option enabled:

![](clip_image001_thumb.png)

and you try to logon to a RDP session (with correct credentials):

![](image_thumb2.png)

you might encounter this error message:

![](image_thumb3.png)

### Client side

Well, if the server allows it, you can temporary disable “Credential Security Support Provider (CredSSP)” in the RPD client. This disables Network Layer Authentication, the pre-RPD-connection authentication, and therefore enables you to change your password via RDP. CredSSP is enabled by default in the RDP client on Windows Vista and forward.

There is no option to disable CredSSP in the RDP client, so here is how you have to do it:

* Start mstsc.exe
* Click Show Options
* Click Save As

![](image_thumb4.png)

* Call it **ChangePassword.rpd** (or anything you’d like, but avoid the name **Default.rdp**)
* Open the saved **ChangePassword.rpd** in Notepad
* Add a new row at the end with the following text:
**enablecredsspsupport:i:0**


![](clip_image003_thumb.png)

* Save the rdp file
* Double-click the rdp file
* Enter the name/IP of a domain connected computer with RDP enabled


Instead of the local Windows Security prompt (the second image in the blog post) you should see a Windows Logon screen on the remote computer (if not, read on anyway):

![](image_thumb5.png)

If the account you log on with at this point has the “User must change password at next logon” option enabled, you get notified about that:

![](image_thumb6.png)

By clicking OK, you have the possibility to change the password.
After changing the password you get confirmation about the change.

Clicking OK logs you in.

([Source: https://mssec.wordpress.com/2015/12/26/forced-password-change-at-next-logon-and-rdp/](https://mssec.wordpress.com/2015/12/26/forced-password-change-at-next-logon-and-rdp/)