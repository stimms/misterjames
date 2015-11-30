title: Getting Your Build Server Ready for VS 2015
tags:
  - MVC5
  - MVC6
  - Visual Studio 2015
id: 7022
categories:
  - MVC6 Conversion
date: 2015-07-24 22:33:57
---

If you’re modernizing your project, one of the things you’ll surely want to do is to make sure that your build server is upgraded to support VS 2015\. Regardless of what CI engine you’re using, there will be at least a little bit of effort required to get your project building again.

_In this series we’re working through the conversion of an MVC 5-based application and migrating it to MVC 6\. You can track the entire series of posts from the _[_intro page_](http://jameschambers.com/2015/07/upgrading-a-real-world-mvc-5-application-to-mvc-6/)_._

For the purpose of this exercise, we’re using TeamCity to run our builds based on a VSC checkin. We’ll get TeamCity prepped to run our build and then update our repository so that we show our build status indicator on the readme home page.

## The TL;DR Details

Here’s the basics of what was required to get the builds back online:

*   Backup and upgrade TeamCity
*   Allow the agents to upgrade, or upgrade them manually
*   Install .NET 4.6 and the [VS 2015 tools](https://www.microsoft.com/en-us/download/details.aspx?id=48159)
*   Ensure that build targets live on your build agents
*   Run your build

## Upgrading the Server

I engaged my teammate [James Allen](http://www.clear-measure.com/our-team/) here to help with some best practices, namely getting the server backed up. You can either back up the TeamCity data from the web interface or one of the other [recommended approaches](https://confluence.jetbrains.com/display/TCD9/TeamCity+Data+Backup), or you could snapshot your server for a reset should one be required. During this process, it’s a good idea to spin down your build agents so that you’re not wrecking anyone’s builds.

Next, we needed to move to version 9.1 of TeamCity, so we ran the [upgrade process](https://confluence.jetbrains.com/display/TCD9/Upgrade) via the web site. This is a painless task and takes only a fraction of the time it took to back up the data. Failing any troubles (we saw none), your build server should be back online in no time, and the build agents were notified (and complied!) to update themselves as well.

[![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image_thumb5.png "image")](https://jcblogimages.blob.core.windows.net/img/2015/07/image13.png)Next, I downloaded and installed the .NET 4.6 installer and the VS 2015 tooling, which can be found on the [VS 2015 download page](https://www.visualstudio.com/downloads/download-visual-studio-vs). You’ll need to explore through the available downloads on the page, as you can see on this screenshot, to grab the relevant files. 

These installs will need to be run on every build agent.

One thing to note was that my original attempt to get the build running failed because of missing build targets at an expected location. I ended up having to copy files from my local machine, where Visual Studio 2015 is installed, from the path: C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v14.0 on the build server.

UPDATE: With the availability of the MS Build Tools for Visual Studio 2015 you no longer have to manually zip up and copy the files. You can download the [MS Build Tools 2015 here](https://www.microsoft.com/en-us/download/details.aspx?id=48159).

## Verifying the Install

You’ll know the tools have been installed correctly if you return to the build configuration settings and add a new build step for msbuild (you don’t have to save it). You’ll see that you’ll have the new options in place:

![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image14.png "image")

The build server should be good to go now! For us, we’re not using an MSBuild build runner, our application is build with a PowerShell script via a batch file. This allows our build to be executed locally with only a small parameter change, and the CI process is entirely encapsulated in code (and under source control).

Provided your project is pointing at the repository, you’ll have a good shot at running the build at this point. For our project, everything worked as expected.

![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image15.png "image")

## Showing Some Bling

Now it’s time to beef up our repo, at least a little. What I’m talking about is wearing our CI on our sleeve, letting everyone on the team (or other watchers of the repository) that our builds are healthy or, perhaps, needing some love; let’s display the build status indicator on our readme, like this:

![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image16.png "image")

First, drill into the build configuration and locate the advanced options under “General Settings”. You need to enable the status widget. 

![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image17.png "image")

Also, from this screen, take note of your Build Configuration ID. This is important because you’ll need to include it in the server request to generate the badge.

Finally, include the following markdown, which is essentially a formatted link with an image inside of it:
<pre class="csharpcode">Current Build Status [![](http://YOUR_SERVER/app/rest/builds/buildType:(id:YOUR_BUILD_CONFIGURATION_ID)/statusIcon)](http://teamcity/viewType.html?buildTypeId=btN&amp;guest=1)</pre>

<style type="text/css">.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }
</style>Be sure to replace the obvious placeholder tokens with your own information.

## Next Steps

With our build server updated and our builds back online, it’s time to start shifting our targets. In the next post, we’re going to update our projects and recover from any errors/challenges we may discover along the way.

Happy coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/07/wlEmoticon-smile3.png)
