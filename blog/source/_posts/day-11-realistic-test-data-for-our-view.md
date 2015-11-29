title: 'Day 11: Realistic Test Data for Our View'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3441
categories:
  - Code Dive
date: 2014-06-11 10:04:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

If a controller deals with a certain type of data – say, entities of the Person type – the index will typically contain a collection of data of that type, or perhaps some other related interface (such as search or filter capabilities, or partial views of related data). Let’s add a PersonController and get a feel for what that looks like.

## Creating the Person Controller

Back on [Day 3](http://jameschambers.com/2014/06/day-3-adding-a-controller-and-view/) we added a SimpleController to our project. Use the same approach to create a new, empty controller now called PersonController.&nbsp; The Index method is the only one in the class, but it’s going to need some data.

## Generating Realistic Test Data

Now, we have a couple of options to get data. We can hand-bomb 25 (or 5 or 10 or 100) entries into some kind of text file. We can new up dozens of objects with copy &amp; paste and have 60 of the exact same person. Or, we can use a test data generator while we flush out our application and get realistic test data with minimum effort.&nbsp; Let’s try that!

Go to your Package Manager Console. If you don’t see that window, typically in the bottom of Visual Studio in one of the tabs. If you still don’t see it, try opening it from View –&gt; Other Windows –&gt; Package Manager Console.

Now, let’s install a package that will generate our data for us, called AngelaSmith. Type the following command in the Package Manager Console:
<pre class="csharpcode">Install-Package AngelaSmith -Version 1.0.1</pre>
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

You should see confirmation that the package is installed.

## Configuring AngelaSmith

We’ll need a place to store the data that is generated, so at the class level, add a static field.
<pre class="csharpcode"><span class="kwrd">private</span> <span class="kwrd">static</span> ICollection&lt;Person&gt; _people;</pre>
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

We’re making it static so that we don’t have to recreate the data on each page load, just when the app starts up.&nbsp; Now add a static controller to configure the data generator.
<pre class="csharpcode"><span class="kwrd">static</span> PersonController()
{
    _people = Angie.Configure&lt;Person&gt;()
        .Fill(p =&gt; p.BirthDate)
        .AsPastDate()
        .Fill(p =&gt; p.LikesMusic)
        .WithRandom(<span class="kwrd">new</span> List&lt;<span class="kwrd">bool</span>&gt;(){<span class="kwrd">true</span>, <span class="kwrd">true</span>, <span class="kwrd">true</span>, <span class="kwrd">false</span>, <span class="kwrd">false</span>})
        .Fill(p =&gt; p.Skills, () =&gt; <span class="kwrd">new</span> List&lt;<span class="kwrd">string</span>&gt;() { <span class="str">"Math"</span>, <span class="str">"Science"</span>, <span class="str">"History"</span> })
        .MakeList&lt;Person&gt;(20);
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

It might seem like a lot is going on, but it’s actually quite straightforward. First is a call to the Configure method to enter configuration mode.
<pre class="csharpcode">_people = Angie.Configure&lt;Person&gt;()</pre>
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

Then we have three Fill statements that demonstrate a few of the ways that we can generate data. For example, you can get a date from the past, fill a property with a random value (60% of the time LikesMusic will be true), or, here, use a lambda to set up an anonymous function to populate the value.
<pre class="csharpcode">.Fill(p =&gt; p.Skills, () =&gt; <span class="kwrd">new</span> List&lt;<span class="kwrd">string</span>&gt;() { <span class="str">"Math"</span>, <span class="str">"Science"</span>, <span class="str">"History"</span> })</pre>
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

Finally, we call the MakeList method to get the list of 20 entities. By default, it will generate 25 but you can specify however many you like.
<pre class="csharpcode">.MakeList&lt;Person&gt;(20);</pre>
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

## Adding the Index View

We’ll need to pass the view some data, when it’s ready, so let’s now update our Index method so that it does that.
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult Index()
{
    <span class="kwrd">return</span> View(_people);
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

You’re going to like this part. Add an empty View to the project by right-clicking on the Index method of the Person Controller. Then add the following code:
<pre class="csharpcode">@model IEnumerable<span class="kwrd">&lt;</span><span class="html">SimpleSite.Models.Person</span><span class="kwrd">&gt;</span>

@Html.DisplayForModel(Model)</pre>

Yeup! That’s it! Run your site to see your list of peeps!

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb5.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image19.png) 
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

## Next Steps

Now that we’ve got some data to work with, let’s start exploring the parts of Bootstrap that can be easily used to add some zing to our site. Tomorrow, we’ll create a form that can be used to search our people.