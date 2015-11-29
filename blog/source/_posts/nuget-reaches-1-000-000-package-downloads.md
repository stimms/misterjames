title: 'NuGet Reaches 1,000,000 Package Downloads'
tags:
  - NuGet
id: 1101
categories:
  - Develop Meta
date: 2011-06-22 18:36:00
---

The .Net community is beginning to embrace open source software.&nbsp; For years, we’ve struggled to find a way to elegantly share and use other people’s code – and ultimately leverage billions of lines of code – in a way that fits into our existing toolset and workflow.

NuGet has begun to solve those problems, and now, it’s here in force.

Adoption rate is beginning to grow, new projects are being started with NuGet in the kit and people are beginning to see that there are great gains to make on existing projects as well.

### <font style="font-weight: bold">Now It’s Really Real</font>

This morning, NuGet reached a significant milestone, marking it’s official arrival: 1,000,000 package downloads.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/NuGet-Reaches-1000000-Package-Downloads_70DA/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/NuGet-Reaches-1000000-Package-Downloads_70DA/image_2.png)

Why is this significant?&nbsp; Well, for starters, it means that it’s no longer just folks messing around. People are using it.&nbsp; It’s rapidly approaching 2,000 packages and the diversity is good, if not great.&nbsp; I’ve got three projects already in production that I’ve added packages to – not including dependencies – and I would venture a guess that others have as well. 

The hourly downloads have been trending upwards (save a few spikey days).&nbsp; The 1,000,000 downloads in and of itself isn’t the significant part, but it’s a beacon of what’s happening.

So, what _is_ happening?&nbsp; Well, as developers on the .Net stack, we’ve finally got a tool that makes reusing code efficient.&nbsp; We have the ability to consume and update packages (and with the latest version of NuGet we have the ability to define constraints for updates, as well). We can leverage the great work of other devs.&nbsp; We have open source tools to publish open source libraries, helpers, templates, extensions and more. We can package up pre-configured trials of commercial products, wrap licensing around our content, or give it all away.

Because of all these pieces coming together, we are able to move at our own comfortable pace with no barriers to entry. We can choose to be consumers or contributors to this space.

And NuGet is the critical, now widely-used, cog in the wheel to make it happen.

### <font style="font-weight: bold">More to Come</font>

While NuGet is already fantastic in new projects, existing projects that need some love and smaller enterprise projects, it’s tough to get it fully going in enterprise scenarios.&nbsp; This is the next focus for the folks working on NuGet, and one they’re [taking seriously](http://furl.ca/rh).

Improvements to critical features like NuGet.Server publishing, adding support for authenticated sources, and facilitating CI build products outside the MS domain are all in the works.

### <font style="font-weight: bold">Now It’s On You</font>

For anyone who doubts that you can influence this product, you’re wrong in two ways:

1.  **It’s easy to post problems you run into.** If you find something that doesn’t tick the way you like it, post it on the [project site](http://nuget.codeplex.com). If others are running into the same thing, they will vote on it.&nbsp; If there’s enough traction, it’ll get promoted to a work item and someone will implement it.  <li>**<u>You</u> can implement it**. If it’s a feature/bug/workflow issue that’s really got you hemmed in, you might as well quit yer bitchin’ and get to work. Fork the source, post the patch and request a pull.&nbsp; Done.&nbsp; And then you can go to sleep a less horrible person. 

And, once you get into this and you find out how easy it is to share code, you might get the inkling to share some of yours.&nbsp; It’s free and easy to publish to the [NuGet site](http://nuget.org/), and you can do some pretty interesting things with your packages during the execution of you installation or initialization [scripts](http://oldblog.jameschambers.com/blog/powershell-script-examples-for-nuget-packages).

### <font style="font-weight: bold">Your Turn</font>

There are so many examples out there of [great pacakges](http://oldblog.jameschambers.com/blog/nuget-presentation-follow-up) ready to use for all kinds of project types and scenarios. There is simply no reason for a developer to put off learning how to use this tool, especially because it’s going to take you all of 10 minutes. Folks, that is less time than it will take you to go and find code that you want to use and get it integrated into your project.

Though some folks have been using it for months, others have been hesitant to explore it enough to fully understand what it can do for them. Hopefully, hitting this 1,000,000 package mark will wake them up.

After all, the only effort required to try NuGet in a safe environment is this:

<font size="3" face="Courier">**File –&gt; New Project**</font>

So, go on. I’ll wait here while you do it.