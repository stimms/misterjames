title: Running A Remote NuGet Feed
tags:
  - NuGet
  - Visual Studio 2010
id: 1071
categories:
  - Code Dive
  - Develop Meta
date: 2011-06-30 18:33:00
---

Following the NuGet docs site to setup a NuGet feed won’t get you one if you follow it (as of Jun 30). There’s one missing step in that doc (it’s here at the end of this one), but I’ve added all the steps below so you can walk through for yourself.

I’ve also created a fork and submitted a pull request to include the missing step, so the online docs should be updated soon.

## Setting Up for Eventual Deployment

Create a new site in IIS.&nbsp; 

Edit the Application Pool and make sure the .NET Framework is set to version 4.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_3.png "image")

## Get Your Project Together

Open Visual Studio in Administrator mode (right-click, Run as administrator).

Create a new, Empty Web Application project.

Open the Package Manager Console and at the PM&gt; prompt, type

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_8.png "image")

Let NuGet do it’s sweet, sweet, beautiful magic.

Right-click on your **_solution_** and select Open Folder in Windows Explorer.&nbsp; 

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_9.png "image")

There is a _packages_ directory there that has a half-dozen packages installed for you.&nbsp; Go through your packages directory, and copy the .nupkg files to the clipboard, then return to Visual Studio and paste them in the Packages folder under your project. It only takes a moment to add a few. Again, you’re copying all the .nupkg files from your **solution** (file system) to the Packages directory in your **project** (Visual Studio).

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_thumb_6.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_19.png)

Press CTRL+F5 and run the site. Click on the packages link and verify that your packages are in the feed.&nbsp; Sweet.

## Publish Your Feed

Close the browser and return to Visual Studio.&nbsp; Select the project in Solution Explorer, then click on Build –&gt; Publish from the menu.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_15.png "image")

This is why we ran as admin…we need to deploy to our local IIS.&nbsp; If you’re logged in as admin you should get a successful publish.

Navigate to your site on localhost.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_14.png "image")

Unfortunately, at this point…our feed is empty. Click on the packages link:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_thumb_5.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ad38df3db4ba_D913/image_17.png)

Lamesauce.&nbsp; Okay…so now to fix it.&nbsp; 

## The Missing Step

Go check your packages directory to the published site.&nbsp; Empty, ain’t it?

No worries…you have two choices here:

1.  Paste all your packages into the site directory (where IIS is looking)  <li>Change the build action on all your .nupkg files to “Content” and republish your site. 

&nbsp;

I was originally under the assumption that it was a permissions problem or a DLL version problem due to a couple of comments out on the interwebs.&nbsp; When I had 5 minutes to look at things here this afternoon, it was quickly clear as to what the problem was.&nbsp; 

Hope this helps!