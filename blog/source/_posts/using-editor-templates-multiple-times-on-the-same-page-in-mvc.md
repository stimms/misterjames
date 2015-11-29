title: Using Editor Templates Multiple Times on the Same Page in MVC
tags:
  - Asp.Net MVC
  - Bootstrap
  - FontAwesome
id: 5091
categories:
  - Code Dive
date: 2014-09-26 00:22:20
---

[![banner5](http://jameschambers.com/wp-content/uploads/2014/09/banner5.png "banner5")](https://leanpub.com/bootstrappingmvc "Bootstrapping MVC - THE eBook for developers using Bootstrap on MVC")

In the last post [we created a star rating control](http://jameschambers.com/2014/09/using-font-awesome-in-an-accessible-bindable-star-rating-control/) that can be easily used in model binding and in displaying a record on our page. The rating control used FontAwesome and some CSS hacks to change radio buttons (which are usable from screen readers) into stars. This gave us a reusable rating control in MVC that is accessible and looks pretty darn okay, and meets our needs for model binding.

![image_thumb6](http://jameschambers.com/wp-content/uploads/2014/09/image_thumb6.png "image_thumb6")

But what if you wanted to build out a list of elements that you wanted to rate? Rather than just one movie, you wanted users to be able to rate your whole collection?&nbsp; Understanding the mechanics of the binding engine are important if you want to POST a list of entities back to an MVC controller.

## Getting the Basics

One of the earliest things that attracted me to the MVC Framework was model binding, and the freedom to stop fishing the in the Form object for parameters that may or may not exist. Over time, many of us built libraries of code that we’d suck in to each project so that we could do things like wrap the extraction of a list of checkbox or radio values up into some kind of abstraction. We don’t [need to do that in MVC](http://theycallmemrjames.blogspot.ca/2010/05/aspnet-mvc-and-jquery-part-4-advanced.html).

You’ll recall that a list of checkboxes in HTML is quite straightforward. The simplest approach might be something like this:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">form</span> <span class="attr">method</span><span class="kwrd">="POST"</span> <span class="attr">action</span><span class="kwrd">="Index3"</span><span class="kwrd">&gt;</span>

    <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="checkbox"</span> <span class="attr">name</span><span class="kwrd">="movies"</span> <span class="attr">title</span><span class="kwrd">="The Last Starfighter"</span> <span class="attr">value</span><span class="kwrd">="The Last Starfighter"</span> <span class="attr">id</span><span class="kwrd">="movie1"</span> <span class="kwrd">/&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="movie1"</span><span class="kwrd">&gt;</span>The Last Starfighter<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;&lt;</span><span class="html">br</span> <span class="kwrd">/&gt;</span>

    <span class="rem">&lt;!-- ... --&gt;</span>

    <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="checkbox"</span> <span class="attr">name</span><span class="kwrd">="movies"</span> <span class="attr">title</span><span class="kwrd">="Amelie"</span> <span class="attr">value</span><span class="kwrd">="Amelie"</span> <span class="attr">id</span><span class="kwrd">="movie2"</span> <span class="kwrd">/&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="movie2"</span><span class="kwrd">&gt;</span>Amelie<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;&lt;</span><span class="html">br</span> <span class="kwrd">/&gt;</span>

    <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="submit"</span><span class="kwrd">&gt;</span>Save<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">form</span><span class="kwrd">&gt;</span></pre>
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

And to “catch” that data, we need an action (the target of the form above) with the same name that accepts a List&lt;string&gt; with the name of the checkbox. The value attribute of the checkbox will be added to the list parameter in your action. Your action will look something like the following:
<pre class="csharpcode">[HttpPost]
<span class="kwrd">public</span> ActionResult Index3(List&lt;<span class="kwrd">string</span>&gt; movies)
{
    <span class="kwrd">return</span> View();
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

The model binder allows any of the selected checkboxes to have a landing spot in your list. You don’t have to check for nulls, or iterate through the form collection to discover all that might have been submitted. It’s worth noting here, as well, that if your value attribute were all integers in your form, you could just as easily have the framework bind to a List&lt;int&gt; in your action signature.

So, that’s great for simple values, but…

## …What About Binding Complex Objects?

I’m so glad you asked. ![Smile](http://jameschambers.com/wp-content/uploads/2014/09/wlEmoticon-smile.png)

[![image](http://jameschambers.com/wp-content/uploads/2014/09/image_thumb1.png "image")](http://jameschambers.com/wp-content/uploads/2014/09/image2.png)If we now look back at our rating control, the control itself is actually ready to go as-is, but we’d need to get a bit more code into our view page (not the partial, but more like, the list of the movies) to get it to tick correctly.&nbsp; 

Here’s what we’ll do:

*   Change our index action to return a list
*   Iterate over the list in our view
*   Pass in some secret magic sauce to the HtmlHelper that renders our stars control

We’ll load up some fake data to push to the view. You would typically want to get this data from a database or an API call of some kind. For now, we’ll go with this:
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult Index()
{
    var movies = <span class="kwrd">new</span> List&lt;Movie&gt;
    {
        <span class="kwrd">new</span> Movie{Title = <span class="str">"The Last Starfighter"</span>, Rating = 4},
        <span class="kwrd">new</span> Movie{Title = <span class="str">"Flight of the Navigator"</span>, Rating = 3},
        <span class="kwrd">new</span> Movie{Title = <span class="str">"Raiders of the Lost Ark"</span>,  Rating = 4},
        <span class="kwrd">new</span> Movie{Title = <span class="str">"Amelie"</span>,  Rating = 5}
    };

    <span class="kwrd">return</span> View(movies);
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

Great, now we just need to update our view. Basically, you can take everything out of the form and just replace it with the following, a foreach over the collection we’re passing in:
<pre class="csharpcode">    @foreach (var movie in Model)
    {
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="row"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>@movie.Title<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="hidden"</span> <span class="attr">value</span><span class="kwrd">="@movie.MovieId"</span> <span class="attr">name</span><span class="kwrd">="@string.Format("</span><span class="attr">movies</span>[{<span class="attr">0</span>}].<span class="attr">MovieId</span><span class="kwrd">", i)"</span> <span class="kwrd">/&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="hidden"</span> <span class="attr">value</span><span class="kwrd">="@movie.Title"</span> <span class="attr">name</span><span class="kwrd">="@string.Format("</span><span class="attr">movies</span>[{<span class="attr">0</span>}].<span class="attr">Title</span><span class="kwrd">", i)"</span> <span class="kwrd">/&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>@Html.EditorFor(p =<span class="kwrd">&gt;</span> movie.Rating, "StarRating", string.Format("movies[{0}].Rating", i++))<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
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

Also, don’t forget to change the @model on your page to the collection of Movie as so:
<pre class="csharpcode">@model List<span class="kwrd">&lt;</span><span class="html">YourApplication.Models.Movie</span><span class="kwrd">&gt;</span></pre>
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

Finally, update the code block at the top of the page to declare a variable that we will use as a counter over our collection:
<pre class="csharpcode">@{
    ViewBag.Title = <span class="str">"My Awesome List of Awesome Movies"</span>;
    var i = 0; <span class="rem">// movie control index</span>
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

We’re moving along now! There’s two crucial parts in how all this will work when it gets rendered on the client and eventually hits the model binder on the way back in when the form is submitted. The first is the way I’ve constructed the name attribute on our controls. For instance:
<pre class="csharpcode">name=<span class="str">"@string.Format("</span>movies[{0}].MovieId<span class="str">", i)"</span></pre>
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

In the above code, on the first iteration, the name would be rendered as “movies[0].MovieId”. The prefix “movies” is what our parameter needs to be called on our action (for the POST). The index notation [0] tells MVC which element this object is in our list, and finally, the “MovieId” is simply the property we want to pass in.&nbsp; 

_**Note**: While I’m passing in the ID and the Title as hidden params, I’m only doing so to show the model binder in action. You should always test/cleanse these values in your controller to prevent over-posting of data, where a malicious user could modify the object without your consent._

The second noteworthy point is the overload that I’m calling on the EditorFor method:
<pre class="csharpcode">@Html.EditorFor(p =&gt; movie.Rating, <span class="str">"StarRating"</span>, <span class="kwrd">string</span>.Format(<span class="str">"movies[{0}].Rating"</span>, i++))</pre>
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

That’s important because I’m passing the similarly generated name (as above) as the name of the HTML field name. Remember that [in the last post](http://jameschambers.com/2014/09/using-font-awesome-in-an-accessible-bindable-star-rating-control/) the EditorTemplate contained references to the ViewData.TemplateInfo.HtmlFieldPrefix? That is normally provided for you by default, but in the case of a list of objects, you need to provide it on your own. That code, in the Shared\EditorTemplates folder, was referenced like this:
<pre class="csharpcode">var htmlField = ViewData.TemplateInfo.HtmlFieldPrefix;</pre>
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

…and it was sprinkled throughout the control template.

Finally, we’re going to need to get that POST action in line.&nbsp; Update your controller method to look like this:
<pre class="csharpcode">[HttpPost]
<span class="kwrd">public</span> ActionResult Index(List&lt;Movie&gt; movies)
{
    <span class="kwrd">return</span> View(movies);
}</pre>

Now, when the form is submitted each set of indexed form fields will be used to new up an instance of our Movie class and placed in the collection. I’ve set a breakpoint so that you can see it in action:

![image](http://jameschambers.com/wp-content/uploads/2014/09/image3.png "image")
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

You’d obviously want to do more than just cycle the movies through the controller, so rather than just returning the list you could do your validation, update your database, or aggregate stats from this response to a running average. So much fun to be had.

## Summary &amp; Next Steps

I always like to encourage folks to try things out and see if they can get it working on your own. It all starts with File –&gt; New Project. Be sure to check out my [Bootstrap series](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/) or purchase [a copy of my book](https://leanpub.com/bootstrappingmvc) for more examples like this, and happy coding!