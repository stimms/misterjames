title: Bootstrapping Asp.Net MVC–Introduction to Twitter.Bootstrap.Mvc4
tags:
  - Asp.Net MVC
  - Bootstrap
id: 1631
categories:
  - Code Dive
  - Develop Meta
date: 2013-02-02 15:54:12
---

I’m continuing my series in exploring Bootstrap integration with the Mvc Framework – part of the Asp.Net stack – and in this entry we’re going to be looking at a helper package that you can install in seconds to kickstart a project.&nbsp; Covered in this video are:

*   Installing from NuGet  <li>Forking from GitHub, building locally and using the latest bits (or contributing!)  <li>Navigation routes  <li>Auto-scaffolded views for CRUD operations<iframe height="315" src="http://www.youtube.com/embed/qSY2kVmZuhs" frameborder="0" width="560" allowfullscreen></iframe> 

## Getting the Latest Stable Bits

One thing I didn’t mention in the video is that you don’t have to build the packages locally to get the latest versions. Eric Hexter has a MyGet feed setup to help you out in this regard. [![image](http://jameschambers.com/wp-content/uploads/2013/02/image_thumb.png "image")](http://jameschambers.com/wp-content/uploads/2013/02/image.png)From the Package Manager Console in Visual Studio, simply pop open the options and add a new package feed:
 > [http://www.myget.org/F/erichexter/](http://www.myget.org/F/erichexter/ "http://www.myget.org/F/erichexter/") 

This lets you install the package from MyGet if you want to expose features not yet posted to the primary NuGet feed.

Alternatively, you can use the NuGet Package Explorer (as I am in the image shown here) to browse through the packages on the feed. Primarily you’re going to be looking for “Bootstrap” or “Navigation” in the feed to pick up the relevant packages.

## Detail All the Things!

I cover a lot of ground in the video and try to give you a sense of what is possible to do in only a few short minutes. If you want to dive deeper here are some resources to help you out as you explore Twitter’s Bootstrap library working with Asp.Net Mvc:

*   GitHub repo for [Twitter.Bootstrap](https://github.com/twitter/bootstrap)*   The repo for the [Twitter.Bootstrap.Mvc4 project](https://github.com/erichexter/twitter.bootstrap.mvc)<!--EndFragment--> <li>Asp.Net Mvc tutorials [here on my blog](http://jameschambers.com/tag/asp-net-mvc/)  <li>Asp.Net Mvc [home page](http://www.asp.net/mvc)  <li>An overview of [Post-Redirect-Get](http://en.wikipedia.org/wiki/Post/Redirect/Get)  <li>The rest of my series on [Bootstrapping Mvc](http://oldblog.jameschambers.com/blog/bootstrap-asp.net-mvc-quickhits-installing-bootstrap) 

Have fun!