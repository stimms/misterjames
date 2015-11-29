title: Real World Scenarios Working with NuGet
tags:
  - CodeProject
  - NuGet
  - Visual Studio 2010
id: 1191
categories:
  - Code Dive
  - Develop Meta
date: 2011-06-01 18:50:00
---

If you’re working with ASP.NET and haven’t made the jump to MVC yet, you have to admit it’s getting harder and harder to _not_ commit.&nbsp; We’ve got some pretty juicy tools on this side of the fence with MVC3, especially when coupled with NuGet and some of the 1600+ open source packages that are already available free of charge.

## Installation, Adding and Upgrading Packages

When you use an “Install-Package” command to install a package NuGet runs off first to grab the lastest version of the package from the repository. It “installs” it in the packages folder under your solution:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_2.png)

It adds entries to your packages.config file in your **target project**:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_4.png)

It may modify your web.config files…

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_thumb_4.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_10.png)

…add project references…

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_8.png)

…or files…

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_thumb_5.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_12.png)

…before completing.&nbsp; And if there are other dependencies in the package, those are downloaded and installed as well.&nbsp; You will be notified if there are any conflicts.

Updating is a little more tricky because there is no auto-resolution from the console to force all required dependencies to upgrade.&nbsp; To see what I mean, try installing Ie9ify to a new MVC3 project.&nbsp; You have to go through:

*   an error and rollback on the ie9ify package install…it’s requesting jQuery 1.5.2  <li>you attempt to update jQuery, but it fails because of dependencies  <li>you have to update each package separately (jQuery UI, Validation and vsdoc)  <li>then you update jQuery  <li>then you can install ie9ify 

Now, you can pass in an –IgnoreDependencies flag, but that doesn’t really help get you out of the water; you’ll still need to update all the other packages and resolve the conflicts manually.

## Multiple Projects

Note that if you have more than one project on a solution that NuGet has to have a target for where its action will play out, and I’ve found that this will be the first project added to the solution.&nbsp; So, for example, if you create a blank solution, add a models project and then add an MVC project, you’ll need to retarget your Package Manager Console.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_thumb_6.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-NuGet-with-Multiple-Projects_7C58/image_14.png)

So, not always a necessity, but worth looking at if a package install/update goes south.&nbsp; You will notice this blatantly if you try to update or uninstall a package when you are targeting the wrong project. 

There are legitimate reasons to target different projects at different times, so this is a great feature to have.&nbsp; It is confusing, however, to have NuGet autocomplete package names for you when you try to tab them out; all packages installed in the solution will appear in the list.

## Cleaning Up Post-Uninstall

Actually, I’m surprised at how good a job NuGet does at cleaning up.&nbsp; For instance, if the package adds entries to the project’s app.config or web.config files it will properly remove them for you as well.&nbsp; Same goes if it adds files to your project; they, too, will be removed.&nbsp; Binary references, same deal.

There are some instances where an uninstall will report a clean job, but you’ll still need to do a little work.&nbsp; Here are a few examples:

*   If you rename a file that’s been added by a package, it will remain in your project  <li>You can change the value component of web.config and app.config files and NuGet will still remove them cleanly, but if you change the name/key of the entry, it’ll stick around  <li>Dependencies aren’t removed when you remove a package, even if they were installed with the package in the first place. 

## Rounding Up

While there is still some room to grow, NuGet has become quite the little workhorse in the last few builds.&nbsp; With the power of Power Shell behind the scenes most third party integration can be automated by package contributors.&nbsp; Not only is there a growing library of community-sourced packages out there, but you create your own as well, or even create your own repository internally and take advantage of easy project setup company wide.

Questions about NuGet? Just ask! I’d be happy to help work through anything you’re trying to tackle.

[CodeProject](http://anyurl.com)