title: 'Day 7: Semi-Automatic Bootstrap – Display Templates'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3271
categories:
  - Code Dive
date: 2014-06-07 10:40:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

On [Day 6](http://jameschambers.com/2014/06/day-6-reusing-design-elements-on-multiple-pages/) we looked at the idea of using a partial view to make it easier to render certain types of data more easily. We can go even further, and allow the scaffolder and HTML helpers to render what we’re looking for without the need for the additional calls to render partials.

If you provide the MVC Framework with a template to render a type and give the right kind of hints to your class properties you’ll be duly rewarded with ease of effort in bringing a rich UI to the client. For now, we’ll start with the display end of things.

## Bootstrap Styling for Checkboxes

We can really use any type that we want, but I’m going to start with Boolean properties. While they seem at first to be straightforward, they also provide a nullable case that we will need to deal with and render appropriately, much like any other type, yet they’re simple enough that we can work through an starter example.

What we’re doing here is creating a Display Template. You can think of it as a partial view in a way, but you usually do a bit more to inspect the properties and interact with data from the view engine in order to properly augment the rendering. We’ll see more of this tomorrow.

Enough chatter…now create a new folder called and located at Views\Shared\DisplayTemplates and create a file in there called Boolean.cshtml, then paste in the following code:
<pre class="csharpcode">@model <span class="kwrd">bool</span>?

@<span class="kwrd">if</span> (Model.HasValue)
{
    <span class="kwrd">if</span> (Model.Value)
    { &lt;span <span class="kwrd">class</span>=<span class="str">"label label-success"</span>&gt;Yes&lt;/span&gt; }
    <span class="kwrd">else</span>
    { &lt;span <span class="kwrd">class</span>=<span class="str">"label label-warning"</span>&gt;No&lt;/span&gt; }
}
<span class="kwrd">else</span>
{ &lt;span <span class="kwrd">class</span>=<span class="str">"label label-info"</span>&gt;Not Set&lt;/span&gt; }</pre>
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

As the view that is responsible for rendering Boolean values, we must first set the model type of the view. I’ve used a nullable bool as it’s more durable and works with either the basic bool or the nullable version.

The rest of the code is fairly straightforward, just determine if there’s a value or not, then set up the label based on the value or lack thereof.

## Using the Display Template

Because we named it “Boolean.cshtml”, the MVC Framework will use our template in favor of the built-in template for Booleans, which you’ll recall from [Day 4](http://jameschambers.com/2014/06/day-4-making-a-page-worth-a-visit/) was simply a checkbox. We saw something like this:

&nbsp;![image](http://jameschambers.com/wp-content/uploads/2014/06/image14.png "image")

But with the new template in place, we’ll see this:

![image](http://jameschambers.com/wp-content/uploads/2014/06/image15.png "image")

You can modify your Index.cshtml at this point to see this in action by adding the following line of code:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>Likes Music: @Html.DisplayFor(model =<span class="kwrd">&gt;</span> model.LikesMusic)<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span></pre>
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

And that’s it, a call to DisplayFor makes the magic happen. From now on, all Boolean values will be rendered as yes/no/not set labels throughout your site. 

## Next Steps

Of course, the display is interesting enough, but what about editing? What if you don’t want all checkboxes to suffer the same disappearing fate? Tomorrow we’ll look at getting the editor in place and to give the MVC Framework more direction on when to use it.