title: 'Bootstrap + Mvc: Controlling Template Rendering'
tags:
  - Asp.Net MVC
  - Bootstrap
  - MVC
  - Visual Studio 2012
id: 671
categories:
  - Code Dive
  - Projects
date: 2012-07-11 17:45:00
---

As we explore Twitter's CSS and JavaScript library in Asp.Net MVC, we're starting to get into the meat of "Bootstrapping" our site, and exploring control templates. If you'd like to keep up with the project code, you can check out the repository on [GitHub](https://github.com/MisterJames/BootstrappingMvc). With a few clean-ups on the create form, our site is now looking like this:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Bootstrap--Mvc-Controlling-Template-Rend_7B1/image_3.png "image")

When you create display and editor templates to override the default templates offered for types by the Mvc Framework, you don't always want those templates used. How do you get around this?
<iframe height="315" src="http://www.youtube.com/embed/EGnsj6zuUcs" frameborder="0" width="560" allowfullscreen></iframe> 

Change the name of your template to something that doesn't match an existing type, then use the UIHint attribute to coerce the property into using your template when being rendered.&nbsp; Nice!&nbsp; Here's a property with the correct attributing:
<pre class="csharpcode">[Display(Name = <span class="str">"VIP - Skip the Line"</span>)]
[UIHint(<span class="str">"SwitchedBoolean"</span>)]
<span class="kwrd">public</span> <span class="kwrd">bool</span> CanSkipLine { get; set; }
</pre>

&nbsp;

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

You can follow the posts in this series from the [main page](http://oldblog.jameschambers.com/blog/bootstrap-asp.net-mvc-quickhits-installing-bootstrap), where I'm curating the links to the instalments of the series.

Cheers!