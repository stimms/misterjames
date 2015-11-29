title: Questions about the ASP.NET MVC 3 Framework
tags:
  - Asp.Net MVC
id: 1001
categories:
  - Code Dive
  - Design
  - Develop Meta
date: 2011-07-26 18:24:00
---

Here is a collection of the most common questions I've run into when working with developers who are new to application development using the ASP.NET MVC 3 Framework.&nbsp; I will continue to expand this list as questions come up, feel free to ask yours in the comments below!

&nbsp;

# Creating New Projects

### <font style="font-weight: bold">What if I don’t like the look of the default project?</font>

You really have two options here; you can edit the _Layout.cshtml file located in Views\Shared\ to make it your own (the Site.css stylesheet is in the Content folder), or you can opt to start with an empty project. You’ll get a very clean _Layout.cshtml at this point, and a really reduced Site.css to work with. Remember that getting your project going is as easy as adding controllers and views, but you won’t have the authentication bits that the Internet Application template gives you.

### <font style="font-weight: bold">Is it okay to use the ASPX View Engine instead of Razor?</font>

Of course! If you’re worried about learning the pattern as well as a new view engine, you can start with a syntax you’re more comfortable with. You will have a harder time finding source samples, however, as most folks on the MVC Framework are using Razor. Also note that there is a different paradigm at play in MVC. ASP.NET Web Forms use a PageController pattern and make use of view state. You don’t have those or page events in the same way in MVC, so don’t be surprised if you don’t find them!

### <font style="font-weight: bold">Why should(n’t) I use HTML 5 semantic markup?</font>

HTML5 and CSS 3 aren’t fully supported on all browsers and the implementations that are there aren’t yet consistent. What’s important to remember is that HTML5 is HTML4 _plus_ an assortment of new features. If you’re targeting HTML5 and using the new features you’ll still need to write alternate views (degrade gracefully) unless you have a fixed deployment environment (like corporate intranet), however, it’s important to remember that the semantic tags _do_ add value to your page today, they _are_ backwards compatible (especially with Modernizr) and are now starting to be better read and understood by search engines.

&nbsp;

# Controllers and Views

### <font style="font-weight: bold">Do controller actions have to return Views?</font>

Not at all! You can return plain text, images, PDFs, page fragments (typically called partial views), XML or JSON…pretty much whatever you like.

### <font style="font-weight: bold">Why isn't a link to my new controller in the Menu?</font>

Navigation isn’t something that can be fully free, but it can be easy. We could create a link in our _Layout.cshtml file, and this would be a great exercise for the reader. If you're working from a clean project, you want to add a third LI element to the NAV’s UL. In the new LI, you can follow the pattern of the other menu items and use the HtmlHelper extension method to render links. Using Visual Studio’s IntelliSense you can figure out which order to pass in the label of the link, the action and the controller you’re looking for. Or just re-read that sentence.

### <font style="font-weight: bold">When I type return View() how does it know which view to return?</font>

This, again, is part of the beauty of convention over configuration.&nbsp; The view that will be returned is the one that has the same name as the method in the controller. If you have a method in the Food controller with the following implementation:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Questions-about-the-.NET-MVC-3-Framework_A869/image_3.png "image")

...then you're going to need a view named SpicyPickles in the Food folder inside of Views.

You're not bound by this, however, and you can return alternate views if you like by passing the name of the view as a string:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Questions-about-the-.NET-MVC-3-Framework_A869/image_6.png "image")

&nbsp;

# Models

### <font style="font-weight: bold">Why are my model classes not showing up in the MVC tooling?</font>

The most likely reason is that you have not built you application. The MVC tooling looks at types in your assemblies. If you haven't built your app since you added those model types to your project (or modified them), the tooling won't pick up all the correct bits for you.

### <font style="font-weight: bold">I'm having trouble binding my model when submitting data to my controller. How do I debug the request?</font>

There's a couple of things you should try to do. First, get a copy of Firebug running (in Firefox) or use the developer tools in IE9 or Chrome.&nbsp; You need to examine the message you're sending to the server. Check to make sure that your parameters are named correctly, conform to the correct type and are actually capturing the correct values from the client. Set a breakpoint in your controller and examine the object. Make sure the correct action is being invoked.&nbsp; [Glimpse](http://getglimpse.com/) is also coming a long way in revealing how your request is seen by the server and can easily be installed via NuGet.

&nbsp;

# Authentication

### <font style="font-weight: bold">Do I have to use the ASPNETDB.MDB that is created for me for authentication?</font>

No, you can setup your own database following [these steps](http://oldblog.jameschambers.com/blog/setting-up-an-alternate-data-source-for-your-asp.net-authentication-provider).