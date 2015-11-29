title: 'Day 16: Conceptual Organization of the Bootstrap Library'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3871
categories:
  - Code Dive
date: 2014-06-16 10:49:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

There are three main areas of what the Bootstrap people refer to as the infrastructure of the library, and provided you’ve tackled the [basics](http://jameschambers.com/2014/06/day-15-some-bootstrap-basics/) and [intent](http://jameschambers.com/2014/06/day-5-bootstrap-for-the-asp-net-developer/), you should have all you need in those areas of the documentation to markup pages to your heart’s content. Today, we’re just going to look at those areas so you can get a feel on where to hunt for things.

This series is by no means meant to teach you CSS basics or how to modify Bootstrap’s core, rather, it’s about leveraging Bootstrap from MVC. However, there are important bits you should have a feel for, especially if the idea of tinkering with Bootstrap’s internals is up your alley.

## The Base CSS

Some HTML elements of the library are just “free”. You get defaults for BODY, background colors, FORMS and form elements without adding any classes. for example.&nbsp; Others need only simple class assignment, such as styling “lead” copy on a page with the P tag.

[Buttons in Bootstrap](http://getbootstrap.com/css/#buttons) use classes, which seems odd at first but they work on BUTTON, A and INPUT elements so once you learn the classes this is rather simple to apply.&nbsp; Each element requires the base “btn” class, and if you want additional coloring you can use one of the contextual classes (active, success, info, warning or danger).
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-warning"</span><span class="kwrd">&gt;</span>Warning<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span></pre>

There are a number of scenarios where these contextual colors can be applied to backgrounds with tables or paragraphs, and be used for text as well, mostly documented in the [helper classes](http://getbootstrap.com/css/#helper-classes) section of the site.&nbsp; You’ll also find info on using these under component- or element-specific docs, such as with the [table](http://getbootstrap.com/css/#tables).
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

There is more information on the basics, as well as diving into the code for customization, on the [CSS documentation page](http://getbootstrap.com/css/). If you are familiar with [SASS](http://getbootstrap.com/css/#sass) and [LESS](http://getbootstrap.com/css/#less), or want to [trim down pieces of Bootstrap](http://getbootstrap.com/getting-started/#customizing) that are not required for your site check out those links as well. 

## Bootstrap Components

When Bootstrap requires more structure to put things together you’ll be looking to the component documentation. The [navbar](http://getbootstrap.com/components/#navbar) we looked at yesterday, for example, needs specific containers, classes and arrangements in order for it to render properly.

Dropdowns, button groups, input groups and combinations thereof are all possible with the right markup. Adding things like breadcrumbs, pagination controls, badges and labels with consistent styling has always been a thorn in my side, but they’re made easy with Bootstrap.

You can dig further into these elements and more on the [Components page](http://getbootstrap.com/components/).

## Adding Functionality with JavaScript

It’s worth noting that most everything up to this point work without the JavaScript library. However, adding the JS to your site really starts to make Bootstrap sing. (You’ll also need it to handle specific use cases like navbars with collapsed regions.)

One of the things I like most about Bootstrap is that it considers the CSS markup the first-class API, meaning you don’t have to write any JavaScript to make Bootstrap components like modals or tabs work. They’ve pushed this away into their JS file, and allow you to trigger related behaviors with data attributes, like here with [the alert](http://getbootstrap.com/javascript/#alerts). By simply adding a button to your alert element, you get dismiss capabilities for free:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="close"</span> <span class="attr">data-dismiss</span><span class="kwrd">="alert"</span> <span class="attr">aria-hidden</span><span class="kwrd">="true"</span><span class="kwrd">&gt;</span><span class="attr">&amp;times;</span><span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span></pre>
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

The docs [JavaScript controls](http://getbootstrap.com/javascript/) are a little light, and don’t offer much for explanation, but you can learn a fair bit by experimenting with the data attribute classes or using the JS API directly.

## Next Steps

Tomorrow is going to be a quick one with a special announcement on some _free_ upcoming training on the MVC Framework and live interaction with experts in the field!

After that, we’ll get back to using these Bootstrap components as something to augment our user experience for applications built with the MVC Framework.