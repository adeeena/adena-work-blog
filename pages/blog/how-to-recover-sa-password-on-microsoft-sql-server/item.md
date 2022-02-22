---
title: 'How To Recover SA Password On Microsoft SQL Server'
media_order: kristaps-ungurs-5mGpTvl4tfI-unsplash.jpg
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
---

There are some specific scenarios specially when you manipulate the DOM with `ng-repeat` directive. Lets imagine we have a code that prints a list of element using an `ng-repeat`. In your controller you have a code to instantiate a bootstrap tooltip for all the elements that are going to be rendered when the `ng-repeat` is compiled by AngularJs. As a best practice it is better to place this type of code in a directive, for this example purpose, we are going to keep it simple and leave it in the controller.

===

The problem here is that the code within the controller is executed before angular compiles the `ng-repeat`, before the DOM elements are part of the HTML. It is hard if not impossible, to find a place to bind events to the elements of an `ng-repeat`, since angular doesn't provide any event that fires when the elements finish rendering. Here we are going to share haw to create an event `onFinishRender` directive.

I created this simple directive to capture the moment that the last element is inserted in the DOM,  with this we will be able to call a JavaScript method after the ng-repeat completes the rendering process.

This is the view:

>     <ul>
>       <li ng-repeat="item in items" on-finish-render="callMyCustomMethod()">
>           dummy Text
>       </li>
>     </ul>

Here is the `onFinishRender` directive:

>     .directive('onFinishRender',['$timeout', '$parse', function ($timeout, $parse) {
>         return {
>             restrict: 'A',
>             link: function (scope, element, attr) {
>                 if (scope.$last === true) {
>                     $timeout(function () {
>                         scope.$emit('ngRepeatFinished');
>                         if(!!attr.onFinishRender){
>                           $parse(attr.onFinishRender)(scope);
>                         }
>                     });
>                 }
>             }
>         }
>     }])

We going to inject the $timeout service to emit an event and include it within the angular digest process, and the `$parse` service, to include our method within the scope when the `ng-repeat` finished;

Basically what we going to do is get when the `scope.$last` variable is equal `true`, this mean that this is the last time the `ng-repeat` going to render an element.

Then we going to send an event with `scope.$emit('yourCustomEventName')`.

Then we going to parse the method within the scope.

In case we want to capture the event in the controller, we can use the `$scope.$on()` function.

>     //this is the code to capture the emited event
>     $scope.$on('ngRepeatFinished', function(ngRepeatFinishedEvent) {
>                 //you also get the actual event object, ngRepeatFinishedEvent
>                 //do stuff, execute functions -- whatever...
>      });

### Mirror from
[jomendez - ng-repeat onFinishRender directive AngularJs - http://www.jomendez.com/2015/02/05/ng-repeat-onfinishrender-directive-angularjs/](http://www.jomendez.com/2015/02/05/ng-repeat-onfinishrender-directive-angularjs/)

[Archive.is mirror - https://archive.vn/y7fWL](https://archive.vn/y7fWL)



