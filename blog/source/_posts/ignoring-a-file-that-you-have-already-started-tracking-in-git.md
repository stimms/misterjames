title: Ignoring a File That You Have Already Started Tracking in Git
tags:
  - Git
id: 651
categories:
  - Develop Meta
date: 2012-09-14 17:42:00
---

> These are the confessions of GitHub for Windows user who is new to Git. I'm using GitHub to make code available from my demonstrations, presentations and workshops and am generally enjoying Git for source control. 

Today I put in about 30 minutes (and a few too many commits) into trying to prevent AuthConfig.cs from being committed to my project on GitHub. AuthConfig.cs is a new file in the basic Asp.Net MVC 4 template that allows for seamless OAuth integration with sites like Facebook and Twitter. I need the file in the repository, but I don't want to share my API keys.

As a side note, I’ve long [been a fan of Asp.Net authentication and authorization](http://theycallmemrjames.blogspot.ca/2011/01/built-in-authentication-and.html) and the recent overhaul is sexy with OAuth awesomesauce. Check out my link at the end of this post to see what I mean.

## Just Trying to Git It

Yes, I'm still a relative rookie to Git and there are some things that I haven't wrapped my head around, but most of the time I at least have the vocabulary I need to find out what was going on.
 > <font color="#434343">**EDIT: **Ready a rookie! You don’t have to ignore if you’re telling Git to “assume-unchanged”.</font> 

First off, ignoring doesn't work on files that are already tracked. This means that if you've created a file before you've added your ignore rule, then Git is already "tracking" the file and it doesn't matter if you try to add an ignore rule after the fact.&nbsp; At this point - which is where I was - <strike>you need to add your ignore rule and then</strike> instruct Git to assume that the file is unchanged.

<strike>I added the following line to my ignore file, which is easily accessed through the GitHub for Windows UI, under settings.</strike>
<pre class="csharpcode">      # Membership
      AuthConfig.cs</pre>
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
</style>

[![image](http://jameschambers.azurewebsites.net/wp-content/uploads/2012/12/image_thumb.png "image")](http://jameschambers.azurewebsites.net/wp-content/uploads/2012/12/image3.png)

The last step was to tell Git that we don’t really want to submits changes anymore.&nbsp; To do this, right-click on your repo in GitHub for Windows and select “Open a Shell Here”:

[![image](http://jameschambers.azurewebsites.net/wp-content/uploads/2012/12/image_thumb1.png "image")](http://jameschambers.azurewebsites.net/wp-content/uploads/2012/12/image4.png)

Finally, type this in the shell that opens up, replacing the project name and path as appropriate to your AuthConfig.cs file:
<pre class="csharpcode">    git update-index --assume-unchanged YourProject\App_Start\AuthConfig.cs</pre>
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
</style>

This is really just a 2 second process, but I wish I knew it 30 minutes ago. Hope this helps out!

## Link-o-liciousness

*   [Asp.Net OAuth Integration](http://www.asp.net/vnext/overview/videos/oauth-in-the-default-aspnet-45-templates) in the default templates with Scott Hanselman<li>The documentation on update-index, [straight from Git](http://git-scm.com/docs/git-update-index)<li>GitHub for Windows [info and download page](http://windows.github.com/)