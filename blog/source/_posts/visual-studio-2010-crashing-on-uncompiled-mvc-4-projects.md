title: Visual Studio 2010 Crashing on Uncompiled MVC 4 Projects
tags:
  - Asp.Net MVC
  - Visual Studio 2010
id: 771
categories:
  - Code Dive
date: 2012-04-15 17:55:00
---

I have come across an issue where in MVC 4 pilot project, while working in views, the IDE would crash fairly frequently. I am using Visual Studio 2010 with SP1 and we are using GitHub for our source control.&nbsp; Luckily I came across this article:

[http://www.asp.net/whitepapers/mvc4-release-notes#_Toc303253806](http://www.asp.net/whitepapers/mvc4-release-notes#_Toc303253806)

In the Known Issues and Breaking Changes section, it lists this gem:
 > **After installing ASP.NET MVC 4 Beta, the CSHTML/VBHTML editor in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML editor may pause for a long time after typing snippet or JavaScript inside cshtml or vbhtml files.** This occurs only in ASP.NET MVC 4 applications which have just been created and have not yet been compiled.  <p>The workaround is to compile the project to get the assemblies in the bin folder. Note, if you clean the project which removes the assemblies from the bin folder, the editor problem will come back.  <p>This will be corrected in the next release. 

This was fairly similar to my case, but not entirely correct.&nbsp; We are using NuGet package restore on our projects, and our .gitignore file is set up to exclude binaries.&nbsp; So, even if you do compile the project, we’re not checking the bins into source control.&nbsp; The IDE was crashing as though the project had never been build.

Thankfully the workaround still applies, and building the project after merging from upstream still did the trick.