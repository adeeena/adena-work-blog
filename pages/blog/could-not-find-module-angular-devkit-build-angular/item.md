---
title: 'Could not find module “@angular-devkit/build-angular”'
date: '08:25 04-09-2020'
taxonomy:
    category:
        - javascript
        - typescript
        - angular
    tag:
        - Angular
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

After updating to Angular 6, I get the following error on ng serve:

>     Could not find module "@angular-devkit/build-angular" from "/home/Projects/myProjectName".
>     Error: Could not find module "@angular-devkit/build-angular" from "/home/Projects/myProjectName".
>         at Object.resolve (/home/Projects/myProjectName/node_modules/@angular-devkit/core/node/resolve.js:141:11)
>         at Observable.rxjs_1.Observable [as _subscribe] (/home/Projects/myProjectName/node_modules/@angular-devkit/architect/src/architect.js:132:40)

`ng update` says everything is in order. Deleting `node_modules` folder and a fresh `npm install` install did not help either.

===

Install `@angular-devkit/build-angular` as dev dependency. This package is newly introduced in Angular 6.0.

>     npm install --save-dev @angular-devkit/build-angular

or,

>     yarn add @angular-devkit/build-angular --dev


### Mirror from
[StackOverflow - Could not find module @angular-devkit/build-angular - https://stackoverflow.com/questions/50333003/could-not-find-module-angular-devkit-build-angular](https://stackoverflow.com/questions/50333003/could-not-find-module-angular-devkit-build-angular)