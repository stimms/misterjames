title: Bootstrap Icon Classes as Strings
tags:
  - Bootstrap
  - 'c#'
id: 1801
categories:
  - Code Dive
date: 2013-02-23 21:52:33
---

I’m working on the [Twitter.Bootstrap.Mvc](https://github.com/erichexter/twitter.bootstrap.mvc) package with [Eric Hexter](https://twitter.com/ehexter) and one of the things that we’re going to be doing is templating around forms and controls as an extension to the [NavigationRoutes](https://github.com/erichexter/NavigationRoutes) project that we’ve broken out.&nbsp; We’re going to be adding support to include automatic prepending/appending of icons to known form elements. 

This has led me to break out an old class I had created a while ago which exposed all the Glyphicons used in Twitter.Bootstrap as strings.&nbsp; I just created the gist (located [here](https://gist.github.com/MisterJames/5021502)) which looks something like this:
<script src="https://gist.github.com/MisterJames/5021502.js"></script> 

It’s nothing terribly complicated, but it might help if you don’t want to sling the code yourself.