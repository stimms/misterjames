title: 'Cloudy With a Chance of Mobile: Mvc Framework, Windows Azure and Windows Phone'
id: 761
categories:
  - Uncategorized
date: 2012-04-18 17:54:00
tags:
---

Interested in having a web site that supports user-specific data? Perfect, use Asp.Net and the built in membership providers. Want to put it in the cloud? Sweet, jump on the Azure bandwagon. Want to easily define your data model and store it?&nbsp; No worries, EF Code First will make you cry, you love it so much. Want to access that data from Windows Phone? Well, my friend, you've come to the right place. I will take you from start-to-finish in creating an application that encompasses all of these pieces: 

1.  An Asp.Net Mvc 4 web site with a Web API controller to serve up some data. This data will be protected by the built-in Asp.Net membership providers.  <li>A local Service Deployment using the Windows Azure SDK.&nbsp; This will be how we host the Mvc web site &amp; Web API.  <li>A Windows Phone 7.1 client that accesses the protected data using the Asp.Net membership provider. 

&nbsp;

## Getting Tooled Up

We need to have the right kit in place to make this happen.&nbsp; Please make sure you have the tools below if you want to follow along:

1.  Visual Studio 2010 with [SP1](http://www.microsoft.com/download/en/details.aspx?id=23691) installed  <li>An internet connection.&nbsp; Oh, wait...  <li>The [latest Azure bits](http://www.windowsazure.com/en-us/develop/net/) downloaded and installed  <li>The Windows Phone 7.1 [SDK](http://www.microsoft.com/download/en/details.aspx?id=27570) setup on your machine  <li>Laurent Bugnion's [MVVM Light Toolkit](http://mvvmlight.codeplex.com/) 

&nbsp;

## Bootstrapping the Projects

I use two instances of Visual Studio to develop in scenarios like this as we'll need elevated privileges to run the Compute Emulator for Windows Azure and I like to be able to leave my web project running while I work on my Windows Phone client application. 

Launch the first IDE in _Adminstrative_ mode.&nbsp; Create a new project from the "Cloud" templates and select Windows Azure Project.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/728684ba0d70_1428D/image_3.png "image")

Next, we're prompted to add our new role project, we'll select MVC 4:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/728684ba0d70_1428D/image_12.png "image")

Finally, we'll pick Internet Application from the available templates in the Mvc 4 templates:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/728684ba0d70_1428D/image_11.png "image")

Now, simply run the application by pressing F5.&nbsp; Windows Azure needs a moment to start up, then you'll see the web site launch. Proceed to register in the site so that you have a valid log in. 

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/728684ba0d70_1428D/image_10.png "image")

Start your second IDE up, pick File –&gt; New Project and navigate to the Silverlight for Windows Phone templates. Pick the vanilla Windows Phone Application project, and when you're prompted be sure to target Windows Phone 7.1.&nbsp; 

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/728684ba0d70_1428D/image_15.png "image")

You can quickly run your phone app to get your emulator running by pressing F5.

## Next Steps

At this point, though we're not doing too much that is interesting, we have our two core projects up and ready to go. We created an Mvc 4 web role inside our Azure project and created a user account that we will late use to log in from the phone.&nbsp; Next, we created the shell of our Windows Phone project that will hold a simple UI to display data that we load from the Mvc site.&nbsp; With very little effort, we have our two major components in place and will be able to quickly advance our project.

Stay tuned for the next article where I will show you how to define a simple model and expose it via Web API.