title: 'Project K, dnvm, dnx, & dnu and Entity Framework 7 (for bonus points)'
tags:
  - ASP.NET
  - Asp.Net MVC
  - Entity Framework
  - Visual Studio 2015
id: 6401
categories:
  - Code Dive
date: 2015-05-04 12:58:48
---

Things, they are a-changing!

[![image](https://jcblogimages.blob.core.windows.net/img/2015/05/image_thumb.png "image")](https://jcblogimages.blob.core.windows.net/img/2015/05/image.png)If you’ve played with different versions of ASP.NET 5 and MVC 6 along the way and have recently updated to the RC build of Visual Studio 2015 you’ll likely have noticed a few changes.&nbsp; Just a couple.

I found that I’ve been mostly able to survive the transition with a few questions around runtimes and where things have moved, but not all the bits are obvious. Here’s a list of some things that you’ll likely want to know as you navigate the M-V-Seas.&nbsp; (See what I did there? ![Smile with tongue out](https://jcblogimages.blob.core.windows.net/img/2015/05/wlEmoticon-smilewithtongueout.png) )

A big thanks goes out to fellow Canadian [Simon Timms](https://twitter.com/stimms) and beach bum [Dave Paquette](https://twitter.com/Dave_Paquette) for stumbling through these bits with and for me. #experts

## Renaming and Reorganization

There used to be two separate commands for language services and running/booting up a site or application. These commands (k and klr) have both been merged with the runtime environment and are all now part of dnx, a.k.a. the .Net Execution environment.

The version manager, which was previously the kvm script (ps1 or sh, depending on environment), is now a command line utility called dnvm, or .Net Version Manager.

Finally, a new utility has been added that replaces the previous dependencies manager with a few enhancements (such as command hoisting from your project.json). The new name is dnu (the .Net Development Utility) and you use it to restore or manage packages, create NuGet packages or publish your project.

So, in summary:
<pre class="csharpcode">k, klr, kre  =&gt; dnx
kvm          =&gt; dnvm
kpm          =&gt; dnu</pre>
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

## So You Want to Run a Migration?

We used to use the package manager console in Visual Studio to do our migrations work, however, this is not currently the case in VS2015\. I imagine that this will continue to improve, but there is still a delta in the way that we used to do things. Today, we’re going to do things a little differently: in a properly prepared console, you’ll type the following in your **project directory** (not your solution directory):
<pre class="csharpcode">dnx . ef [options] [command]</pre>
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

This command tells the .Net execution environment to use the current directory and to run the ef command. From there you could type migration or whatever else you’re looking for. Leaving the options and command out, for instance, gives you the magic unicorn of awesome.

## Wait, That Didn’t Work?

First of all, you’ll need to make sure that you have the tooling for the dn* utilities on your PATH. There needs to be environment variables setup to point you to the correct runtimes, or rather, the runtimes you’re currently targeting. You can see all the runtime versions you have installed by typing:
<pre class="csharpcode">dnvm list</pre>
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

Typically, you’ll see two different **_runtimes_** (clr and coreclr) for each architecture (x64, x86), and you’ll see each of those for each **_version_** you have installed.

The “correct” version for your purposes may be a moving target, so make sure you have a runtime and version that works with the version of EF you have. If you’re not sure (or you thought you were sure but things aren’t working) take to [Jabbr](https://jabbr.net/#/rooms/EntityFramework) and ask for a hand (they are great there).

Next, your solution and/or project will have to have the correct references to EF. Edit your project.json to have the following dependencies and commands (you can do this in VS or Notepad or whatever tool you like, just save the file when you’re done):
<pre class="csharpcode">  <span class="str">"dependencies"</span>: {
    <span class="str">"EntityFramework.SqlServer"</span>: <span class="str">"7.0.0-beta4"</span>,
    <span class="str">"EntityFramework.Commands"</span>: <span class="str">"7.0.0-beta4"</span>,
    ...
  },

  <span class="str">"commands"</span>: {
    <span class="str">"ef"</span>:  <span class="str">"EntityFramework.Commands"</span>,
    ...
  },</pre>
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

Almost there. Now we need to restore those packages locally so that you can use the EF tooling. To do that, we’re going to use the following command from the **solution directory**:
<pre class="csharpcode">dnu restore</pre>
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

Voila! You should be good to go! Navigate to your **project directory** and hack away at your migrations. 

## Keeping Up to Date

So, go grab Visual Studio 2015! If you run into trouble there is a wealth of information out there (albeit, much of it is quickly becoming outdated or conflicting). As I already mentioned, [Jabbr](https://jabbr.net/#/rooms/EntityFramework) is a great place to ask questions, as are Twitter and Stack Overflow. [Brice Lambson](https://twitter.com/bricelambs) periodically posts updates on his [blog](http://www.bricelam.net/). I have found that the documentation for Asp.Net 5 has also been kept fairly up-to-date, which you can [read here](http://docs.asp.net/en/latest/).

Happy coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/05/wlEmoticon-smile.png)
