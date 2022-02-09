---
title: 'Getting the ng-object selected with ng-change'
taxonomy:
    category:
        - angularjs
    tag:
        - Coding
        - Javascript
        - AngularJS
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

>     <select ng-options="size as size.name for size in sizes" 
>     ng-model="item" ng-change="update()"></select>


Then in your `update()` method, `$scope.item` will be set to the currently selected item.

And whatever code needed `item.size.code`, can get that property via `$scope.item.code`.