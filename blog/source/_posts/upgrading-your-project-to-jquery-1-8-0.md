title: Upgrading Your Project To jQuery 1.8.0
tags:
  - jQuery
  - jQuery UI
id: 661
categories:
  - Code Dive
date: 2012-09-03 17:44:00
---

If you are using jQuery UI on your project and you update jQuery to 1.8.0 you’ll also want to grab the latest bits of jQuery UI (which is, at the time of this post, sitting at 1.8.23).&nbsp; This is an important maintenance release, particularly if you’re using any jQuery UI components that make use of position.

For our project, it was the slider control as well as autocomplete that revealed the problem.

The jQuery library folks had marked $.curCSS as deprecated as of jQuery 1.4, but they still had it in at least 12 spots in jQuery UI 1.8.20\. With the 1.8.0 release of the jQuery, $.curCSS is no longer there and you’ll start to get errors such as:
 > Object doesn't support property or method "curCSS" 

Or, similarily:
 > $.curcss is not a function >  

Moving to jQuery UI 1.8.23 will fix this for you. There is a post [here](http://blog.jqueryui.com/2012/08/jquery-ui-1-8-23/) explaining some of the details and a very brief [changelog](http://jqueryui.com/docs/Changelog/1.8.23) that details the position bits.