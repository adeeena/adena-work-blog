---
title: 'How To Recover SA Password On Microsoft SQL Server'
media_order: 'sql-server-1.png,sql-server-2.png,sql-server-3.png,sql-server-5.png,sql-server-6.png,sql-server-7.png,sql-server-8.png,sql-server-9.png,sql-server-11.png,sql-server-12.png,sql-server-13.png,sql-server-10.png,sql-server-4.png'
date: '14:47 07-09-2020'
taxonomy:
    category:
        - JavaScript
        - AngularJS
        - 'custom directive'
    tag:
        - AngularJS
        - Programming
        - JavaScript
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
hero_image: markus-winkler-BHB3G8iIQrA-unsplash.jpg
---

If an SA password is ever lost then instead of re-installing SQL and re-attaching the DB, SQL Server provides a great disaster recovery method that keeps objects and data in the master DB. Members of the server’s local Administrators group can reach SQL Server by starting it in single-user mode.

In this blog we will show how to recover SA password on Microsoft SQL server by starting SQL Server in single user mode when you forget `sa` credentials.

===

### Pre-requistiques
* Microsoft SQL server configured with mixed (Windows and SQL).
* User should be administrator of server.

### Steps
* Open the SQL Server Configuration manager. 
![sql-server-1](sql-server-1.png "sql-server-1")

* Go to SQL Server Services and Stop the SQL Server Instance. 
![sql-server-2](sql-server-2.png "sql-server-2")

* Right click on SQL Server Instance and select the Properties option. 
![sql-server-3](sql-server-3.png "sql-server-3")

* Go to Startup Parameters tab, type `–m` to Specify a startup parameter and click on Add 
![sql-server-4](sql-server-4.png "sql-server-4")

* Click on Apply and then OK.
![sql-server-5](sql-server-5.png "sql-server-5") 

Note: If SQL server is before 2014 we could not change startup parameter but you can check the start parameter go to Advanced and check the startup parameter.


* Click OK on Warning which prompts after apply the changes in Startup Parameter. 
![sql-server-6](sql-server-6.png "sql-server-6")

* Now, Start SQL Server Instance.
![sql-server-7](sql-server-7.png "sql-server-7")

* Open the SQLCMD command prompt.
![sql-server-8](sql-server-8.png "sql-server-8")

* Run the command `EXEC sp_addsrvrolemember ‘DOMAIN\Username (Domain username) ‘, ‘sysadmin’;`
* Run the command `GO`.
![sql-server-9](sql-server-9.png "sql-server-9")

* Now go to SQL Server Instance and click on Properties.
![sql-server-10](sql-server-10.png "sql-server-10")

* Go to Startup Parameters tab and remove startup parameter `–m` from Existing parameters after that click on Apply then OK.
![sql-server-11](sql-server-11.png "sql-server-11")

* Now stop the SQL instance again.
![sql-server-12](sql-server-12.png "sql-server-12")

* Now Open the Microsoft SQL server management.
* Use the same account which has been added above to login.
* Now expand the SQL server > Security > Logins and open the properties to reset SA account password.
![sql-server-13](sql-server-13.png "sql-server-13")

* The login account added is a SQL sys admin now, so it can reset the SA password.



### Mirror from
[How To Recover SA Password On Microsoft SQL Server](https://www.hex64.net/blog/how-to-recover-sa-password-on-microsoft-sql-server/)



