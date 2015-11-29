title: 'Day 1: The MVC 5 Starter Project'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 2801
categories:
  - Code Dive
date: 2014-06-01 10:56:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Getting started in MVC is the easiest it’s ever been, and it’s even easier to look good doing it. Rather than building up yet another short-lived, likely passing design, Microsoft made another nod to open source software and adopted the Bootstrap library as a design language. We’ll get to all those bits shortly, but for now let’s see what happens when you get started in MVC 5.

First off, select File –&gt; New Project from the Visual Studio 2013 menu.&nbsp; If you select the “Web” category you’ll see only one type of project, an “Asp.Net Web Application”. All web projects have been consolidated under the “One Asp.Net” banner, so the project type is an easy selection. The next dialog that pops up gets a little more interesting:

![image](http://jameschambers.com/wp-content/uploads/2014/06/image.png "image")

You’ll note that there are a few more options here. Click around on the project templates to see the description (we’re going to be focusing here on MVC). The notable options outside of your template selection are the Authentication, Core References and Azure options.

**Authentication** – If you’re targeting a general web audience you’ll likely want to make use of Individual User Accounts (the default), which opens the door for you to integrate with 3rd party providers such as Microsoft, Facebook, Twitter and Google. If this isn’t the route for you, click through and read about each of the other types.

**Core References** – With the removal of project type GUIDs came the arrival of much less hacking to get different types of web projects working in the same solution. You can now easily put Web Forms, MVC and Web API all in the same project. Also here is your ability to add a test project.

**Windows Azure** – This is the part I like, streamlined deployment baked directly into the project creation. While you can always change these options or even add them later, for those of you who have an Azure account, this is an easy way to get code running quickly in the cloud. If you don’t have an account, it’s easy to set one up.

## Running the Project 

Well, don’t wait! Just run the site! You’ll land on a welcome page (just a static HTML page in your project) but you can hit CTRL+F5 to see your wonderful new digital bits running locally.

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image1.png)

Bang. You got yourself a web site. Let’s pause to consider what’s going on.

The page that is delivered to the browser is straight HTML – image, script, p tags and all. In a nutshell, here’s how that content gets rendered to the client:

*   A request for a specific resource (a page, in this case) hits the server and comes into the pipeline.  <li>A controller is selected, based on the routing configuration.  <li>Any dependencies for the controller are resolved and the controller is instantiated.  <li>Any payload from the request – form content, query string – is evaluated to see if it can be used to provide parameters for the method (action) that will be executed.  <li>The MVC Framework finds the most appropriate action to call based on the payload and method (POST, GET for example) of the request.  <li>The method is invoked, passing in the discovered parameters.  <li>The information you need (if any) is passed through to Razor, the default view engine.  <li>Razor prepares to render the CSHTML file (or VBHTML file) as requested by the framework using anything that was built up so far.  <li>Rendering occurs, which often and typically includes a “layout” or master page that is wrapped around the entire content.  <li>Happiness ensues. 

There are actually a few more moving parts involved in the request as we’ll see in the coming weeks, and we haven’t even talked about application startup yet. The framework also provides a number of ways to intercept requests and participate in the executing pipeline, so we’ll cover that as well as the month unfolds.

On day 2, we’ll breakdown the solution that was created and start twiddling some bits. See you tomorrow and happy coding!