---
title: 'C# - How to load XML from URL on XmlDocument()'
taxonomy:
    category:
        - 'c#'
        - 'useful extensions'
        - coding
        - xml
    tag:
        - Programming
        - 'C#'
        - 'C# Useful extensions'
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

I have this code:

>     string m_strFilePath = "http://www.google.com/ig/api?weather=12414&hl=it";
>     
>     XmlDocument myXmlDocument = new XmlDocument();
>     myXmlDocument.LoadXml(m_strFilePath);
>     
>     foreach (XmlNode RootNode in myXmlDocument.ChildNodes)
>     {
>     }

but when I try to execute it, I get this error :

**Exception Details: System.Xml.XmlException: Data at the root level is invalid. Line 1, position 1.**

Why? Where am I wrong? And how can I fix this problem on C#?

Also tried with :

>     myXmlDocument.Load(m_strFilePath);    

but I get :

**Exception Details: System.Xml.XmlException: Invalid character in the given encoding. Line 1, position 503.
**

===

### Solution

It's telling you that the value of `m_strFilePath` is not valid XML. Try:

>     string m_strFilePath = "http://www.google.com/ig/api?weather=12414&hl=it";
>     XmlDocument myXmlDocument = new XmlDocument();
>     myXmlDocument.Load(m_strFilePath); //Load NOT LoadXml

However, this is failing (for unknown reason... seems to be choking on the `à` of `Umidità`). The following works (still trying to figure out what the difference is though):

>     var m_strFilePath = "http://www.google.com/ig/api?weather=12414&hl=it";
>     string xmlStr;
>     using(var wc = new WebClient())
>     {
>         xmlStr = wc.DownloadString(m_strFilePath);
>     }
>     var xmlDoc = new XmlDocument();
>     xmlDoc.LoadXml(xmlStr);


### Mirror from
[StackOverflow - How to load XML from URL on XmlDocument() - https://stackoverflow.com/questions/7496913/how-to-load-xml-from-url-on-xmldocument/7496968#7496968](https://stackoverflow.com/questions/7496913/how-to-load-xml-from-url-on-xmldocument/7496968#7496968)

