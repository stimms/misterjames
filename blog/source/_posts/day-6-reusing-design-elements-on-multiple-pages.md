title: 'Day 6: Reusing Design Elements on Multiple Pages'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3201
categories:
  - Code Dive
date: 2014-06-06 10:53:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

While it’s important to learn the mechanics of the underlying technology we use, it’s never really much fun to have to do the same work over and over again.  That’s part of why we’re here; rather than hacking out the CSS for each site or page, we use frameworks and libraries to bring a uniform look-and-feel.

Likewise, we want to make sure we’re leveraging the MVC Framework to do the heavy lifting for us when we’re trying to get our data out to the client.

## Layouts and Partial Views

Back on Day x I briefly mentioned the layout page, located in your Views\Shared directory.  This type of “master page” gives you the ability to create a template that can be shared on all pages where you want to wrap your content with common elements, such as side bars, menus, footers and the like, or likewise, to stuff in client-side application scripts, style sheets and other elements.

At the other end of the layout page is the concept of a partial view – a template that can be used to render a specific kind of content in different pages. You may also hear them called by the shortened name of “partials”. This allows you to define one time how a set of data will be rendered and then include the partial wherever you need to render the content.

## Creating a Partial View

Let’s consider for a moment the section of labels that is rendered at the bottom of our person page that lists the skills.

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb2.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image13.png)

That block of skills is really just a collection of strings that we are displaying as labels decorated with Bootstrap styling.  I could imagine that would be handy to have in other places on my site as well.

Let’s look at that code again quickly and see what we have going on.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
        Skills:
        @foreach (var skill in Model.Skills)
        {
            <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="label label-primary"</span><span class="kwrd">&gt;</span>@skill<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
        }
    <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
<style type="text/css"><!--
.csharpcode, .csharpcode pre
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
--></style>Essentially, we’re dumping a bunch of span elements to the page inside of a p tag, all wrapped up in a div. Not too complex. There is a title to describe the list of labels, and a for..each loop that walks over the collection to dump the labels to the page.

Let’s cut that code to the clipboard and paste it into a new file in the Views\Simple folder called _StringCollection.cshtml. You can add the file by right-clicking on the Simple folder and selecting Add –&gt; View. When prompted, make sure to tick the “Create as Partial View” so that the scaffolder doesn’t set the layout or title, and select the “empty (without model)” template. Using the underscore as a prefix is a convention to let others (and remind yourself) that the file is intended be rendered as a partial view.

Paste the code into the file and update it a little so that the entire partial matches the following code, including the model definition at the top:
<pre class="csharpcode">@model ICollection<span class="kwrd">&lt;</span><span class="html">string</span><span class="kwrd">&gt;</span>

<span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
        @ViewBag.ListDescription
        @foreach (var value in Model)
        {
            <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="label label-primary"</span><span class="kwrd">&gt;</span>@value<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
        }
    <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
<style type="text/css"><!--
.csharpcode, .csharpcode pre
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
--></style>Notice that rather than just writing out “Skills” to the page ahead of our labels, we’re now using a property from the ViewBag. Now, return to your Index.cshtml and, in the place of the code that you removed, add the following lines to your view:
<pre class="csharpcode">@{ 
    ViewBag.ListDescription = <span class="str">"Skills:"</span>; 
    Html.RenderPartial(<span class="str">"_StringCollection"</span>, Model.Skills);
}</pre>
<style type="text/css"><!--
.csharpcode, .csharpcode pre
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
--></style>Run your newly updated page and see…well, you’ll see exactly what we had before, but we’re doing it a little more cleverly!

Here we’ve set the title and then asked Razor, the view engine, to process the partial view file with the contents of the string collection we’ve passed in.  From now on, anywhere in our site, we can push a collection of strings into a label set with a title in two lines of code.

## Next Steps

This isn’t the only way that we can render a type of data with some kind of enhanced templating. Tomorrow we’ll look at making it even more effortless to get our data all fancied up for the ball.