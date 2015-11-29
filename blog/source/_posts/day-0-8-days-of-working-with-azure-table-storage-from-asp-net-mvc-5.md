title: 'Day 0: 8 Days of Working With Azure Table Storage from ASP.NET MVC 5'
tags:
  - 8DaysOfTableStorage
  - Asp.Net MVC
  - Azure
  - Table Storage
id: 5231
categories:
  - Code Dive
date: 2015-01-14 03:59:35
---

Here's a short and simple way to get started using Azure Table Storage from ASP.NET MVC 5\. Following these brief, task-oriented articles will help you learn the mechanics of working with table storage and give you some pointers on how you might want to approach it in your app.

We're going to start with the basics, a simple MVC app with a controller that builds the table, inserts a couple of rows and then displays them.

If you want to follow along, please [pop into Azure](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119) and make sure you’ve got an account ready to go. The trial is free, so get at it!

But then we're going to expand on that, adding other operations to manipulate the data as well as the tables themselves, and then we'll take it to the next level where we start to apply some better strategies around how we actually access the data.&nbsp; After all, MVC is all about separation of concerns!

Most of these principles will apply if you need to access a storage table from other areas of .NET as well…you could just as easily apply these to Web API, a console application or your next WinForms project. By the end, we’ll have extracted the important parts out into a reusable block of code that runs equally as well with the Azure Storage Emulator in your development environment as it does in production in the cloud.

## Working With Azure Table Storage – Our Agenda

*   Day 1: [The Basics of The Basics with Azure Table Storage](http://jameschambers.com/2015/01/day-1-the-basics-of-the-basics-with-azure-table-storage/ "Day 1 &ndash; The Basics of the Basics with Azure Table Storage")
<li>Day 2: [Adding Entities to a Storage Account Table](http://jameschambers.com/2015/01/day-2-adding-entities-to-a-storage-account-table/)
<li>Day 3: [Extracting a Service to Interact with Table Storage](http://jameschambers.com/2015/07/day-3-extracting-a-service-to-interact-with-azure-table-storage/)
<li>Day 4: Manipulating Entities in a Storage Account Table
<li>Day 5: Deleting Entities from a Storage Account Table
<li>Day 6: Using Projection with Dynamic TableEntity and EntityResolver
<li>Day 7: Table Management in your Storage Account
<li>Day 8: Cloud Configuration and Deployment

Check back for more on the series in the days ahead, all the links will be posted here.

And hey, if you’d like to ramp up in your MVC skills, please check out my recent book:

[![banner5](http://jameschambers.com/wp-content/uploads/2014/09/banner5.png "banner5")](https://leanpub.com/bootstrappingmvc "Bootstrapping MVC - THE eBook for developers using Bootstrap on MVC")