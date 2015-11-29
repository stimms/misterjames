title: Embedding Videos With Windows Live Writer and MetroPress
id: 1501
categories:
  - Uncategorized
date: 2012-12-31 13:43:44
tags:
---

## The Problem

Windows Live Writer, IMO, is still the best little tool out there for creating content for blogs. It does a great job at nearly every task related to editing post, pages, categories and tags, and it provides flexible posting options with the ability to manage multiple blogs.

But, at times, she’s verbose! Embedding a YouTube video, for instance, inserts the following code into your post:
<pre class="csharpcode">

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">id</span><span class="kwrd">="scid:5737277B-5D6D-4f48-ABFC-DD9C333F4C5D:befb91d4-acb8-41d9-a0da-ec64375d826f"</span> <span class="attr">class</span><span class="kwrd">="wlWriterEditableSmartContent"</span> <span class="attr">style</span><span class="kwrd">="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"</span><span class="kwrd">&gt;</span><span class="kwrd">&lt;</span><span class="html">embed</span> <span class="attr">src</span><span class="kwrd">="http://www.youtube.com/v/uA7Z8WfD3bU?hd=1"</span> <span class="attr">type</span><span class="kwrd">="application/x-shockwave-flash"</span> <span class="attr">wmode</span><span class="kwrd">="transparent"</span> <span class="attr">width</span><span class="kwrd">="448"</span> <span class="attr">height</span><span class="kwrd">="252"</span><span class="kwrd">&gt;&lt;/</span><span class="html">embed</span><span class="kwrd">&gt;&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
</pre>
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

Unfortunately, that breaks [MetroPress](http://metropress.codeplex.com/) because the embed tag and script cannot load inside a Windows Store app.

The fix is quite simple: rather than using the built-in “Insert Video” feature, you simply need to copy and paste the embed code from YouType, which looks more like this:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">iframe</span> <span class="attr">width</span><span class="kwrd">="560"</span> <span class="attr">height</span><span class="kwrd">="315"</span> <span class="attr">src</span><span class="kwrd">="http://www.youtube.com/embed/uA7Z8WfD3bU"</span> <span class="attr">frameborder</span><span class="kwrd">="0"</span> <span class="attr">allowfullscreen</span><span class="kwrd">&gt;&lt;/</span><span class="html">iframe</span><span class="kwrd">&gt;</span></pre>

This embed code is snapped from Share –&gt; Embed on the YouTube site under your video.

[![image](http://jameschambers.com/wp-content/uploads/2012/12/image_thumb3.png "image")](http://jameschambers.com/wp-content/uploads/2012/12/image7.png)
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

You can see that the new embed code is cleaner, and better still it is allowed in a Windows Store app and the video plays embedded in your posts just fine.

[![image](http://jameschambers.com/wp-content/uploads/2012/12/image_thumb2.png "image")](http://jameschambers.com/wp-content/uploads/2012/12/image6.png)

## A Little Background

MetroPress is a handy way to augment your blog with an offline-capable reader for your followers.&nbsp; From their site:

> In a nutshell, this framework is a flexible app template that can be easily customized and extend it to create more awesome Windows store applications.

Using the quickstart, it takes about an hour to get your blog prepped and the project built up so that you can release a Windows Store (Windows 8) application.&nbsp; Mine passed certification on the first attempt.

You can get MetroPress on their [CodePlex project site](http://metropress.codeplex.com/).