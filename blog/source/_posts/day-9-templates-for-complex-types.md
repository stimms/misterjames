title: 'Day 9: Templates for Complex Types'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3341
categories:
  - Code Dive
date: 2014-06-09 10:55:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Sites like Facebook and Twitter or any site with a “feed” often present their data in a stream of “cards” or “blocks” that are repeatable and consistent, regardless of where you see the snippet of data. Let’s take the UI we created for the Person, augment it a little bit and then make it reusable wherever we might want it displayed.

## Extracting the Person Template

We’re going to build from the idea of using the DisplayTemplates in our Views\Shared folder, and add another file in there called Person.cshtml. Just right-click on the Views\Shared\DisplayTemplates folder and click Add –&gt; View. Name the view Person, use the empty template and create it as a partial.

Now paste the following code into the file:
<pre class="csharpcode">

@model SimpleSite.Models.Person

&nbsp;

<span class="kwrd">&lt;</span><span class="html">hr</span> <span class="kwrd">/</span><span class="kwrd">&gt;</span> 
<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="row"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="col-md-2"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">img</span> <span class="attr">src</span><span class="kwrd">="http://placehold.it/150x150"</span> <span class="kwrd">/&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="col-md-8"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">h3</span><span class="kwrd">&gt;</span>@Model.FirstName @Model.LastName<span class="kwrd">&lt;/</span><span class="html">h3</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>@Model.FirstName was born on @Model.BirthDate.ToString("D").<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>Likes Music: @Html.DisplayFor(model =<span class="kwrd">&gt;</span> model.LikesMusic)<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
        @{
            ViewBag.ListDescription = "Skills:";
            Html.RenderPartial("StringCollection", Model.Skills);
        }
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
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

Most of this you should recognize as fairly close to the code we were using in our Index view for the simple controller. Above, I’ve added some columns and a placeholder image so that we can later give users an avatar. But I’m otherwise continuing to use the same elements we were building off of before, such as the [DisplayFor](http://jameschambers.com/2014/06/day-7-semi-automatic-bootstrap-display-templates/) and [RenderPartial](http://jameschambers.com/2014/06/day-6-reusing-design-elements-on-multiple-pages/) bits we’ve been working on. 

## Updating the Index View

The end result is that our Index view can be _much _simpler than it was. Here is what you need for the whole view source of Index.cshtml:
<pre class="csharpcode">@{
    ViewBag.Title = "Index";
    var likesMusic = Model.LikesMusic ? "active" : null;
    var notAMusicFan = !Model.LikesMusic ? "active" : null;
}

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="page-header"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">h1</span><span class="kwrd">&gt;</span>Welcome to the Person Page <span class="kwrd">&lt;</span><span class="html">small</span><span class="kwrd">&gt;</span>Read all about @Model.FirstName<span class="kwrd">&lt;/</span><span class="html">small</span><span class="kwrd">&gt;&lt;/</span><span class="html">h1</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>

@Html.DisplayForModel(Model)</pre>
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

The DisplayForModel tries to find a display template that matches the type of model that is passed in, and sure enough, naming our partial view above “Person” and placing it in the Views\Shared\DisplayTemplates folder was all we needed to do to wire that up. It’s another example of “convention over configuration” at work in the MVC Framework.

## Next Steps

I’ve used only one set of layout classes as styles for my person template. Later in the month we’ll explore how to make this even more reusable by introducing classes that allow the content to be rendered on different devices and screen sizes.

The placeholder image is okay, but we’d likely prefer to use an image related to the user. Tomorrow we’ll look at a way to implement an extension method that allows us to drop in a Gravatar image for any user.