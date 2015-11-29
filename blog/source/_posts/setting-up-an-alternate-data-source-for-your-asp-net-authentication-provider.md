title: Setting up an Alternate Data Source for your ASP.NET Authentication Provider
tags:
  - Asp.Net MVC
  - Authentication
id: 1021
categories:
  - Code Dive
date: 2011-07-24 18:27:00
---

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_2.png)The authentication provider that is baked in to ASP.NET for free is a great start to managing users on your site, and gives you full capabilities around roles, profiles and authorization within your application.&nbsp; The default project template for internet applications includes functionality designed around it. I'm a big fan of these free jump-starts as they’re also easy to swap out.&nbsp; For the purposes of this post, I'm using Visual Studio 2010, the ASP.NET MVC Framework and SQL Server 2008 Express.

By default, if you try to create an account without prior configuration, the ASP.NET bits happily go about creating a local MDF for you with all the required tables and procs.&nbsp; This is great, if you never intend to save anything else in your database, you don’t wish to rename it, and you don’t want to use an existing data source, but it’s also not the only way to go about getting a data source for your authentication needs.

If you're scratching your head after creating a user account, wondering where the MDF ended up, look no further than Visual Studio's magic show-me-my-flipping-files button.&nbsp; The data file will be in your App_Data directory, but not included by default in the project.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_thumb_2.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_6.png)

## &nbsp;

## Provisioning Your Own Data Source

Thankfully, the good folks at Microsoft have provided us with a tool to create these tables and procs in our own databases as well.

First, we're going to need to get a landing spot for our data, so fire up SQL Server Management Studio and connect to your favourite database server. This can be any SQL Server edition (including Express, [which is free](http://www.microsoft.com/sqlserver/en/us/editions/express.aspx)), running locally or out on your network.&nbsp; Create a new database, I called mine **NotJustAnAuthDatabase**.

Open a Visual Studio Command Prompt as Administrator and type the following:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_8.png)

While you can pass in arguments from the command line, there's also a wizard that launches in the absence of them.&nbsp; This makes it easy to select the database and apply the authentication data model.&nbsp; 

Step through the wizard.&nbsp; We're going to Configure SQL Server for application services.&nbsp; Notice the option to remove the service information?&nbsp; That’s why I said it's easy to swap!

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_11.png "image")

Next, we specify the server and database name that we want to add authentication support to:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_14.png "image")

Click next a couple times, then finish.&nbsp; Your data source is all setup!&nbsp; You can check your handiwork in SSMS:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_thumb_6.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_16.png)

&nbsp;

## Getting ASP.NET In Tune With Our Work

There are only a couple of small steps left to point our provider at the new database.&nbsp; First, we'll need to adjust our ApplicationServices connection string.&nbsp; Pop back into Visual Studio and open up web.config.&nbsp; You'll need to work out whatever your connection string will be for your server and database. Mine looked like this:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_thumb_7.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Alternate_987F/image_18.png)

Next, you can delete the MDF file that was created for you (if you had one created).&nbsp; Remember to use the Show All Files button to reveal the database if it does not at first appear in Solution Explorer.&nbsp; Oh, and a note here: you're deleting a database.&nbsp; It may hold data, so think that through if you want something in it.

When you re-run your app you will now be attached to the freshly-configured data source for authentication, allowing registration to your new backend.

## A Few Cleanup Points

First, authentication isn't the only thing that's covered in Application Services, there is a lot of goodness baked in there. You might consider [using profiles](http://msdn.microsoft.com/en-us/library/014bec1k.aspx) as well.&nbsp; Once a user is authenticated, you can make use of the very simple authorization attributes in ASP.NET MVC, so there's all kinds of peripheral benefits too, not just the benefit of not writing registration/role/authentication code for users on your site.

Secondly, this isn't new.&nbsp; In fact, the first bits of these services were shared with us in late 2005 when we were introduced to the .NET 2.0 Framework.&nbsp; So why am I posting about it now?&nbsp; As easy as it is to use, as comprehensive as the toolkit seems, and as mature as the components are, I still run into senior developers who have never used it or explored it.&nbsp; File –&gt; New, people! **File –&gt; New!!!**

Another reason is that there are lots of new developers out there who aren't sure by looking at VS2005 docs if that material is relevant any more. Some of it isn't. Some is, but the IDE looks different.&nbsp; It may seem cumbersome at first glance to get all this going, but I'm hoping that by showing you all you have to do is run a wizard and update a string that you'll consider giving it a try.

Finally, just remember that if you're mid-stream on a project and you make a switch out you're going to need to first **back up your data**. Setting up the service components in an alternate location doesn't port your users or roles and if you nuke your existing data store that data will be gone.

Best of luck in your authentication adventures!