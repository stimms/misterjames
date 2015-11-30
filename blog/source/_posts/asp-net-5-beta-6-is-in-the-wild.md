title: ASP.NET 5 Beta 6 Is In The Wild
tags:
  - ASP.NET
  - Asp.Net MVC
id: 7201
categories:
  - Develop Meta
date: 2015-07-28 14:52:38
---

The Beta 6 release of ASP.NET 5 is now available. Run the following command to upgrade from a previous version:
<pre class="csharpcode">dnvm upgrade</pre>

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
</style>After that, a “dnvm list” command will give you the following:

![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image22.png "image")

You can also upgrade dnvm itself with the following command:
<pre class="csharpcode">dnvm update-self</pre><style type="text/css">.csharpcode, .csharpcode pre
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
</style>Which will get you up to the beta 7 version (build 10400) of DNVM.

[![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image_thumb6.png "image")](https://jcblogimages.blob.core.windows.net/img/2015/07/image23.png)

You’ll also need the updated VS 2015 tooling, which is available here (along with the DNVM update tools if you want them seperately): [Microsoft Visual Studio 2015 Beta 6 Tooling Download](http://www.microsoft.com/en-us/download/details.aspx?id=48222).

## Why This is Important

As part of my progression in porting an MVC 5 app to MVC 6, one scenario that I needed support for was to have libraries targeting .NET 4.6 reference-able from a DNX project. MVC 6, up to this point, only supported 4.5.1, which meant that you’d have to roll back your targeting if you were on 4.5.2 or 4.6\. 

Of course, multi-targeting is a better option, but requires the time and capacity to either slave over the old code base and NuGet packaging nuances, or port to the new project format where you have much greater in-project support for targeting multiple frameworks.

## What You Get

As previously detailed by [Damien Edwards](https://twitter.com/DamianEdwards), there are bug fixes, features and improvements in the following areas: [Runtime](https://github.com/issues?utf8=%E2%9C%93&amp;q=user%3Aaspnet+is%3Aissue+label%3Aenhancement+milestone%3A1.0.0-beta6), [MVC](https://github.com/issues?utf8=%E2%9C%93&amp;q=user%3Aaspnet+is%3Aissue+label%3Aenhancement+milestone%3A6.0.0-beta6), [Razor](https://github.com/issues?utf8=%E2%9C%93&amp;q=user%3Aaspnet+is%3Aissue+label%3Aenhancement+milestone%3A4.0.0-beta6), [Identity](https://github.com/issues?utf8=%E2%9C%93&amp;q=user%3Aaspnet+is%3Aissue+label%3Aenhancement+milestone%3A3.0.0-beta6). In addition to supporting .NET 4.6 in DNX, they have also added localization and have been working on other things like distributed caching, which you can [read about here](https://github.com/aspnet/Announcements/issues/43).

## What To Watch Out For

This is still a beta, and there are many moving parts.

Be sure to check out the [community standup today](https://live.asp.net/) and head over to [GitHub](https://github.com/aspnet/) for the announcements on [breaking changes](https://github.com/aspnet/Announcements/issues).

Happy coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/07/wlEmoticon-smile4.png)
