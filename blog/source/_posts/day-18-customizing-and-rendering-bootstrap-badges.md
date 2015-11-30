title: 'Day 18: Customizing and Rendering Bootstrap Badges'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3961
categories:
  - Code Dive
date: 2014-06-18 10:35:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Bootstrap comes with some great styles and base starting points, but it’s not meant to be the be-all and end-all of design. There are often times when a simple CSS tweak can make your site stand apart from others.

## Using the Built-In Badge

Badges are pretty easy to render. You mark up a SPAN with the “badge” class and you’re off to the races.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Registrations <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="badge"</span><span class="kwrd">&gt;</span>19<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;&lt;/</span><span class="html">a</span><span class="kwrd">&gt;</span></pre>
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

That ends up looking something like this:

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image34.png "image")

And if you put it in a navbar, it looks like this:

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image35.png "image")

So it’s an easy way to convey that the user has _something_ that needs to be done, check unread messages, complete some kind of task or, say, approve People registrations. But the white-on-grey is a little bland.

## Non-Intrusive Customizations

I wouldn’t recommend modifying the base Bootstrap.css class itself. If there are incremental updates you would lose the ability to update your project without losing changes, save using a merge tool and sorting out the diff on your own. You would likely be better served to extend Bootstrap with LESS or SASS if that suits your fancy, but there is an easier approach as well that’s as old as CSS itself: just add another stylesheet to your project. Sometimes low-tech is the easiest answer.

What we’d like to do is to get a palette of options like so:

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image36.png "image")

Let’s start by adding a new CSS file to the project. Expand the Content folder in your solution and you should see bootstrap.css. Right-click on the Content folder, and add a new Style Sheet called “bootstrap.custom.css”, then paste in the following code:
<pre class="csharpcode">.badge-danger {
  background-color: #d43f3a;
}

.badge-warning {
  background-color: #d58512;
}

.badge-success {
  background-color: #398439;
}

.badge-info {
  background-color: #269abc;
}

.badge-inverse {
  background-color: #333333;
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

These are just classes styled after the button classes, using the same solid colors for the background. You could event go a step further and introduce hover states (like buttons) or borders (like for labels) depending on your fancy.

Next, we need to get our site aware of this file. If you’re a traditional web developer you might be inclined to update your template or master page and add some markup to include the CSS, but we have a different approach in today’s web: update our bundle.

## Including the File

Navigate to the App_Start folder in your solution and open the BundleConfig.cs file. Update the bundle that includes the bootstrap.css file to the following:
<pre class="csharpcode">bundles.Add(<span class="kwrd">new</span> StyleBundle(<span class="str">"~/Content/css"</span>).Include(
            <span class="str">"~/Content/bootstrap.css"</span>,
            <span class="str">"~/Content/bootstrap.custom.css"</span>,
            <span class="str">"~/Content/site.css"</span>));</pre>
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

If you’ve already got the site running, you’ll want to recompile at this point. You won’t need to in order to make changes to your style sheet, but BundleConfig is only ever run once at startup. Bundles have built-in cachebusters that prevent your CSS from getting stale while you edit it, but you’ll need to do that rebuild just once.

## Badge Up Your Site!

Now you are free to add something like the following to any of your pages:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Registrations <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="badge"</span><span class="kwrd">&gt;</span>19<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;&lt;/</span><span class="html">a</span><span class="kwrd">&gt;</span> 
<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span></pre>
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

One cool but likely expected aspect is that if you put a badge inside of an A tag, as I have above here, the badge is also clickable, so keep that in mind as you’re hacking together your markup.

## Next Steps

Next up we will come up with a practical way to leverage these badges in our site and display them in the navbar.