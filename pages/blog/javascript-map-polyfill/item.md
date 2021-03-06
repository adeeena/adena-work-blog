---
title: 'Javascript Map Polyfill'
media_order: brina-blum-jBxqFcnLY9M-unsplash.jpg
taxonomy:
    tag:
        - javsacript
        - polyfill
hide_git_sync_repo_link: false
hero_classes: text-dark
hero_image: brina-blum-jBxqFcnLY9M-unsplash.jpg
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

Polyfill for Javascript `Map`.

===

>     /**
>      * map-polyfill - A Map polyfill written in TypeScript, unit tested using Jasmine and Karma.
>      *
>      * @author Brenden Palmer
>      * @version v0.0.1-alpha.2
>      * @license MIT
>      */
>     !function(){"use strict";var t;!function(t){var e=function(){function t(t,e){this.index=0,this.map=null,this.done=!1,this.map=t,this.type=e}return t.prototype.next=function(){var t;return this.map.keyArray.length>this.index?("entries"===this.type?t=[this.map.keyArray[this.index],this.map.get(this.map.keyArray[this.index])]:"keys"===this.type?t=this.map.keyArray[this.index]:"values"===this.type&&(t=this.map.get(this.map.keyArray[this.index])),this.index++):this.done=!0,{value:t,done:this.done}},t}();t.MapIterator=e}(t||(t={}));var t;!function(t){var e=function(){function t(){}return Object.defineProperty(t,"MAP_KEY_IDENTIFIER",{get:function(){return"MAP_KEY_IDENTIFIER_OZAbzyeCu3_spF91dwX14"},enumerable:!0,configurable:!0}),Object.defineProperty(t,"MAP_SET_THROWABLE_MESSAGE",{get:function(){return"Invalid value used as map key"},enumerable:!0,configurable:!0}),t}();t.MapConstants=e}(t||(t={}));var t;!function(t){var e=function(){function t(){if(null!==t.instance)throw"Get the instance of the MapSequencer using the getInstance method.";this.identifier=0}return t.getInstance=function(){return null===t.instance&&(t.instance=new t),t.instance},t.prototype.next=function(){return"Map_CJPOYUrpwK_aHBtMHXsTM"+String(this.identifier++)},t.instance=null,t}();t.MapSequencer=e}(t||(t={}));var t;!function(t){var e=function(){function e(){}return e.defineProperty=function(n){var r;if(e.isValidObject(n)===!1)throw new TypeError(t.MapConstants.MAP_SET_THROWABLE_MESSAGE);if("undefined"==typeof n[t.MapConstants.MAP_KEY_IDENTIFIER]){r=t.MapSequencer.getInstance().next();try{Object.defineProperty(n,t.MapConstants.MAP_KEY_IDENTIFIER,{enumerable:!1,configurable:!1,get:function(){return r}})}catch(i){throw new TypeError(t.MapConstants.MAP_SET_THROWABLE_MESSAGE)}}else r=n[t.MapConstants.MAP_KEY_IDENTIFIER];return r},e.getProperty=function(n){return e.isValidObject(n)===!0?n[t.MapConstants.MAP_KEY_IDENTIFIER]:void 0},e.isValidObject=function(t){return t===Object(t)},e}();t.MapUtils=e}(t||(t={}));var t;!function(t){var e=function(){function e(t){void 0===t&&(t=[]),this.map={},this.keyArray=[];for(var e=0;e<t.length;e++){var n=t[e];n&&n.length>=2&&this.set(n[0],n[1])}}return e.prototype.get=function(e){if(this.has(e)===!0){var n=t.MapUtils.getProperty(e);return void 0===n&&(n=String(e)),this.map[n]}},e.prototype.has=function(e){var n=t.MapUtils.getProperty(e);return void 0===n&&(n=String(e)),void 0!==n&&"undefined"!=typeof this.map[n]},e.prototype["delete"]=function(e){if(this.has(e)===!0){var n=t.MapUtils.getProperty(e);return void 0===n&&(n=String(e)),this.keyArray.splice(this.keyArray.indexOf(e),1),delete this.map[n],!0}return!1},e.prototype.set=function(e,n){this["delete"](e);var r;try{r=String(t.MapUtils.defineProperty(e))}catch(i){r=String(e)}this.keyArray.push(e),this.map[r]=n},e.prototype.entries=function(){return new t.MapIterator(this,"entries")},e.prototype.keys=function(){return new t.MapIterator(this,"keys")},e.prototype.values=function(){return new t.MapIterator(this,"values")},e.prototype.forEach=function(t,e){for(var n=0,r=this.keyArray;n<r.length;n++){var i=r[n];e?t.call(e,this.get(i),i,this):t(this.get(i),i,this)}},e.prototype.clear=function(){this.map={},this.keyArray=[]},e}();t.Map=e}(t||(t={}));var t;!function(t){window.Map||(window.Map=t.Map)}(t||(t={}))}();

### Source
[Brenden Palmer - Github](https://raw.githubusercontent.com/brendenpalmer/map-polyfill/master/dist/map.min.js)