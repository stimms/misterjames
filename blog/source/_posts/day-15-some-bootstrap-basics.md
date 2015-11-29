title: 'Day 15: Some Bootstrap Basics'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3831
categories:
  - Code Dive
date: 2014-06-16 00:57:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Through the series so far we’ve started to see what Bootstrap can do, but how does it get there? Why does the default page look like it does? And what are some of these classes we’ve been using?

## The Shell That Makes it Tick

Bootstrap is a responsive grid style and JavaScript library that makes some assumptions about the kind of project you’ll be creating:

*   You care about mobile users  <li>You want a familiar UX for your users  <li>You are targeting clients capable of HTML5, CSS and JavaScript  <li>You like the look and feel of Bootstrap, or you: a) know how to change it, or, b) you have a theme you want to use. 

People, there are no more excuses for using tables for layout! Unless, of course, you have a table of data you want to display.&nbsp; You can use the CSS of Bootstrap on it’s own and make use of the built in styles to control the layout of your pages, accommodating for different screen sizes and device resolutions.

JavaScript isn’t a requirement for your site, but anything beyond the basic CSS will need to support it. In some cases, functionality of simple controls is augmented or simply made possible by virtue of the JavaScript library. What does the JS offer?

*   It provides a programmatic API for components and elements of the page  <li>It establishes custom events for actions unique to the Bootstrap controls  <li>It gives the ability to automatically wire up components on your page – almost akin to “programming” it on the page, but using Data Attributes in your markup.  <li>It provides a no-conflict mode to operate with other client-side frameworks  <li>It allows components with heavier processing costs to be delay-initiated, making your page look and feel faster 

## One Grid to…Line Up Them All

Imagine dividing your page up into 12 columns, and using a row to hold those columns, and putting all your rows into a container. Then, you choose if you want to assume a series of “responsive” widths, or a fluid width that is calculated on the fly to control the size of your columns.&nbsp; In a nutshell, that is what a grid layout system is doing for you. 

Check out these samples from the template. Your home page looks like this in a desktop browser on a larger screen resolution:
[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb18.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image29.png)

But when you scale it down, you get the following without having to completely design an alternate site:

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb19.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image30.png)

The grid capabilities give you many options for how you want to tackle your layout regardless of the user’s viewport.&nbsp; **This is significant because you don’t have to write your site twice**. There was previous emphasis on making a “mobile friendly version” of your site, but the Bootstrap opinion on design is that you should be mobile first. If you’re choosing to build a new site on some greenfield project it is worth spending some time on how the grid breaks down.

The best way to do this is to visit the [Grid Template](http://getbootstrap.com/examples/grid/) sample site to see how the different arrangements of classes work together.

## The MVC Framework’s Home Page

Consider those images above from your project’s home page, and have a look at what’s going on. There is a menu at the top of the page that automatically collapses into a mobile-friendly arrangement when the screen size gets smaller.

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb20.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image31.png)

The code for this navbar control is in the _Layout.cshtml file.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="navbar navbar-inverse navbar-fixed-top"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="container"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="navbar-header"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="navbar-toggle"</span> <span class="attr">data-toggle</span><span class="kwrd">="collapse"</span> <span class="attr">data-target</span><span class="kwrd">=".navbar-collapse"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="icon-bar"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="icon-bar"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="icon-bar"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
            @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="navbar-collapse collapse"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="nav navbar-nav"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>@Html.ActionLink("Home", "Index", "Home")<span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>@Html.ActionLink("About", "About", "Home")<span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>@Html.ActionLink("Contact", "Contact", "Home")<span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>
            @Html.Partial("_LoginPartial")
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
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

There are two DIVs inside the inner container, one the header, the second a collapsable section that is displayed on the same row. On smaller screens, that section disappears in favor of the button in the header. The wiring for all this is done with data attributes. 

The links in the header are either rendered in that row, or in a mobile-friendly stack of links when the navbar is collapsed.&nbsp; 

See those three spans in there? Those use a class called icon-bar from the Bootstrap library that looks like this:
<pre class="csharpcode">.navbar-toggle .icon-bar {
  display: block;
  width: 22px;
  height: 2px;
  border-radius: 1px;
}</pre>
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

While I’m assuming they could have used an image or something from Glyphicon here, this is a very light-weight way to render the now familiar “hey I got more stuff for you to look at” iconography that many mobile users are accustomed to.

The rest of the content on the page is rendered in Index.cshtml inside the Views\Home folder.

![image](http://jameschambers.com/wp-content/uploads/2014/06/image32.png "image")

There are two parts in play here, a jumbotron component and a row with three equally-sized columns.&nbsp; The jumbotron is the grey area with the larger title. It assumes the full width of the container it resides in, and gives a few extra style classes to help make content stand out.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="jumbotron"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">h1</span><span class="kwrd">&gt;</span>ASP.NET<span class="kwrd">&lt;/</span><span class="html">h1</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span> <span class="attr">class</span><span class="kwrd">="lead"</span><span class="kwrd">&gt;</span>ASP.NET is a free...<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="http://asp.net"</span> <span class="attr">class</span><span class="kwrd">="btn btn-primary btn-lg"</span><span class="kwrd">&gt;</span>Learn more <span class="attr">&amp;raquo;</span><span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
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

The three bits of content are basically assembles as follows:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="row"</span><span class="kwrd">&gt;</span>
    <span class="rem">&lt;!-- Three of these --&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="col-md-4"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">h2</span><span class="kwrd">&gt;</span>Getting started<span class="kwrd">&lt;/</span><span class="html">h2</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>Content...<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">class</span><span class="kwrd">="btn btn-default"</span> <span class="attr">href</span><span class="kwrd">="http://..."</span><span class="kwrd">&gt;</span>Learn more <span class="attr">&amp;raquo;</span><span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
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

There are three DIV elements that have the col-md-4 (read: content targeting a minimum of a medium screen resolution and spanning 4 columns). These are all contained in the row DIV, and if they add up to 12 or less (3x4 = 12) then they are almost certain to end up on the same row.

## Next Steps

Be sure to punch around with the grid a little and check out the Views\Home\Index.cshtml source in full (above are just the snippets) to get a good grip on how the layout works. Or, try switching the container class on the _Layout to use container-fluid instead. Lots to explore!

We still have a few basic bits and tips to take care of before we return to the mash up of MVC and Bootstrap, so tune in tomorrow where we close those off!