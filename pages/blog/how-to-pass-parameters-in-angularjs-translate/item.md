---
title: 'How to pass parameters in angular-translate?'
media_order: 'Annotation 2020-07-16 094123.png,Annotation 2020-07-16 094225.png,Annotation 2020-07-16 094542.png'
taxonomy:
    category:
        - angularjs
    tag:
        - Programming
        - Javascript
        - AngularJS
hide_git_sync_repo_link: false
visible: true
hero_classes: text-light
hero_image: IMG-0154.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
content:
    items: '- ''@self.children'''
    limit: '5'
    order:
        by: date
        dir: desc
    pagination: '1'
    url_taxonomy_filters: '1'
bricklayer_layout: '1'
display_post_summary:
    enabled: '0'
---

This page concerns on how to pass parameters on angular-translate.

===

### Source text:
![](Annotation%202020-07-16%20094123.png)

### From codebehind:
>     $translate.instant(translationKey, { 'MyTranslationVariable': Notifications.MessagesCount }

![](Annotation%202020-07-16%20094542.png)

### From HTML:
>     {{ 'translation-key' | translate:{ MyTranslationVariable: Notifications.MessagesCount } }}
     
![](Annotation%202020-07-16%20094225.png)