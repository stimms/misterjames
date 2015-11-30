title: 'Day 2: Examining the Solution Structure'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 2871
categories:
  - Code Dive
date: 2014-06-02 10:58:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Now that we’ve had a chance to see what the default project looks like when running, let’s talk about the parts of the project that make it happen. We talked briefly about controllers, actions and views, so let’s figure out where those come from first before moving on to other important contributors to our project.

## Controllers and Views, and Actions, Too

One of the things that the MVC Frameworks does reasonably well is to follow convention over configuration. That means that with very little effort you can make use of most of the features of the framework without having to set XML, change project configuration or the like…it just “works” out-of-box. Of course, this means that it is also fairly reasonable to assume that it is somewhat opinionated, especially when it comes to tooling. So, there are “right” ways and “choose your own pain” ways to approach development.

Thankfully, controllers and views are easy pieces, you can find them in the apt-named folders, “Controllers” and “Views”, respectfully and consistently across most projects. 

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image2.png "image")

Each controller has a corresponding folder inside of Views to help keep things organized further. If you’re generating views from the built-in tooling, they are created here as well. As the MVC Framework tries to find the view you would like rendered it checks here first and then falls back to the “shared” folder, also part of convention.

**A controller is just a class and actions are just methods.** This is an important detail, so keep that in mind as you develop. While you can use either version of either word and be correct, you’ll gain the favor of the lexicon if you try to use the terms as they are known in the framework. Other extension points, for example, reference these terms and they are good cues to help other developers (including the future version of you) survive your code down the road.

**A view is typically HTML plus some optional code written using the Razor syntax.** We don’t create “web pages” any more, we create applications. The view engine – Razor – using your view files and any supplied data to assemble and output HTML, which is returned to the client who initiated the request.

Of special note is the Shared folder under Views, where you’ll find the layout file (_Layout.cshtml) that is used as a template or “master page” for your site, giving you a way to have pages that follow a similar look-and-feel without having to repeat the same layout instructions on each page.

## Models

These guys are pretty easy: they are just classes. Models will have properties and helper methods. They may reflect data that is stored in the database, data that you wish to store in the database, or input from users. They may store options for users to choose from when inputting their data. There are a lot of things Models can be, but for the most part they are just plain old CLR objects (POCOs). 

Where things get interesting is when you “pass” a model to a view, allowing an HTML page to be rendered based on the properties of the information contained therein. 

## Scripts, Content and Fonts

Any web project is driven by a set of static or semi-dynamic resources such as CSS files, images, fonts and JavaScript sources. The solution gives you some basic structure to help keep these organized, but these are not contributors to the MVC project structure and they do not influence the framework. You are free to rename these and organize these types of files as you wish.

The one caveat would be that the default template (mostly driven from your application startup files) does make use of this structure, as do some of the startup components. If you start to move these around, you’ll also need to update those aspects of your project.

In these folders you’ll find the requisite pieces needed to ship a web site that reflects the Bootstrap design language, namely jQuery, the Bootstrap library and stylesheet, the glyphicons font and a few libraries to assist with browser incompatibilities and functionality polyfills to add capabilities that are missing in out-of-date or non-compliant browsers.

## Application Building Blocks

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image3.png "image")The App_Start folder is likely the most interesting from a code perspective as it provides the wiring to pull your application together. Bundles are a way to reduce and compress script and style resources in a non-lossy fashion (as far as the browser is concerned). Filters allow you to modify the execution pipeline of your application. Identity is the implementation of the built-in local user account manager. Routing allows friendly URLs and custom mapping of resources. And the Startup.Auth file (a partial class) is used to tell your app which types of user identies you’ll be using to pair with your local user accounts.

There’s a lot in that last little paragraph, but we’ll unpack it as we go.

## The Root of all Web

We wouldn’t be complete if we didn’t cover all the bits, including those in the root of our application.

At the top of the solution explorer you’ll see “Properties” and “References”, standard in all .Net applications. These give you access to things like assembly information, the built-in web server configuration and references to external dependencies that you take.

Towards the bottom of the list you’ll see a couple of files that are common on all Asp.Net sites, Global.asax and Web.Config. These give instructions to the MVC Framework, to the Asp.Net runtimes and to IIS itself as to how to execute requests and make use of resources. They allow you to store settings and provide values to libraries and assemblies you might be using. You’ll note that Global’s startup method calls out to some of the startup classes we covered in the last section as well.

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image4.png "image")

Favicon.ico is the image that will be displayed in a browser tab when someone visits your site.

Packages.config is a list of all the packages that are required by your application. If you open&nbsp; a project and do not have these packages installed, Visual Studio will go and fetch them for you when you try to build.

There’s one last class in there, a file named Startup.cs, which configures the authentication bits via a call to the Configure method in our Startup.Auth partial class. This one is interesting because it leverages an OwinStartupAttribute to get invoked before anything else in our app is executed. OWIN is a bigger topic that we’ll review later in this series.

## Next Steps

Now that we have the lay of the land, tomorrow we’ll add a new page to our application so we can see a little more of the plumbing in play.

Happy coding!