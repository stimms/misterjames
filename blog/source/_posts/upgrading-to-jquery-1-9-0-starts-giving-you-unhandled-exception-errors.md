title: Upgrading to jQuery 1.9.0 Starts Giving you Unhandled Exception Errors
id: 1581
categories:
  - Uncategorized
date: 2013-01-29 14:52:14
tags:
---

> I am running Windows 8, Visual Studio 2012 and working with Asp.Net MVC4\. My upgrade was from jQuery 1.8.2 to jQuery 1.9.0 when this error surfaced. 

If you upgrade an MVC project to the latest jQuery bits, you man run into an error similar to the following:
 > Unhandled exception at line 115, column 5 in [http://localhost:0000/Scripts/jquery.unobtrusive-ajax.js](http://localhost:0000/Scripts/jquery.unobtrusive-ajax.js)
> 
> 0x800a01b6 - JavaScript runtime error: Object doesn't support property or method 'live' 

There have been a number of methods that were deprecated in the library some time ago, and now jQuery 1.9.0 is executing on those changes and has removed the methods for good. You can read more about the changes on the [jQuery site](http://blog.jquery.com/2013/01/15/jquery-1-9-final-jquery-2-0-beta-migrate-final-released/).

This doesn’t leave you stranded…the team has graciously provided a migration path for those who want to take advantage of the new library enhancements, but still need to maintain legacy code.&nbsp; The unobtrusive ajax script in our MVC projects are still calling some of these deprecated methods, so we need to take advantage of this offering.

To add backwards compatibility to our project, we simple have to drop into the Package Manager Console and type the command:
`PM&gt; Install-Package jQuery.Migrate ` 

&nbsp;

Tada!