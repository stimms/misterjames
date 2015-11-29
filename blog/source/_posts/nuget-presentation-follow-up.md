title: NuGet Presentation Follow-Up
tags:
  - NuGet
id: 1111
categories:
  - Conferences
date: 2011-06-19 18:37:00
---

I received an email from one of the folks at the NuGet talk last night asking about some of the packages we had a look at and I said I’d post a quick list, along with a few others that were handy to have around.

*   [jQuery](http://nuget.org/List/Packages/jQuery), [jQuery.UI](http://nuget.org/List/Packages/jQuery.UI.Combined) &amp; [jQuery.Validation](http://nuget.org/List/Packages/jQuery.Validation) – these are all in the new MVC3 templates by default, but you can use the Package Manager Console in VS2010 to update them to the latest version (along with the vsdoc file).&nbsp; You can also easily add them to existing projects.  <li>[EntityFramework](http://nuget.org/List/Packages/EntityFramework) (EFCodeFirst) – with the CodeFirst bits now built into EntityFramework we are able to quickly and easily define POCO classes and let the framework do the heavy lifting…kinda like scaffolding for databases.  <li>[Modernizr](http://nuget.org/List/Packages/Modernizr) – old browsers suck, but Modernizr makes them suck a loss less.&nbsp; It also exposes the presence of all the modern functionality of the client’s browser (missing or not) via boolean values, allowing us to degrade our functionality gracefully.  <li>[Glimpse](http://nuget.org/List/Packages/Glimpse) – What? Client side debugging for ASP.NET MVC? Ohhh yeah. Glimpse lets you dive deep on both sides of the request and response, letting you get a ‘glimpse’ of what happened when you made a call.&nbsp; Support for MVC, Ajax now, and more coming soon.  <li>[Pluralizer](http://nuget.org/List/Packages/Pluralizer.Mvc) – A simple but elegant solution that lets you contextually set up strings to be pluralized based on the count of the collection you pass it.  <li>[jQuery.ie9ify](http://nuget.org/List/Packages/jQuery.ie9ify) – Helps you add site pinning features and set up things like notifications, icons and the like.&nbsp; A must have for installed user bases (like intranets) where you can standardize on a browser, or where your user base is large enough that adding extras for those on IE9 is worth your time.&nbsp; 

Also, Scott Hanselman has started an NPoW – “NuGet Package of the Week” – posting series on his blog. You can [check that out here](http://www.hanselman.com/blog/CategoryView.aspx?category=NuGetPOW).

Finally, I fielded a couple of questions about init, install and unistall scripts when creating your own packages for NuGet.&nbsp; Here’s [my post](http://oldblog.jameschambers.com/blog/powershell-script-examples-for-nuget-packages) on script tips and PowerShell commands commonly used in other packages.

Thanks again all for coming out!&nbsp; See you June 24th at HTML5 Fest in Winnipeg!