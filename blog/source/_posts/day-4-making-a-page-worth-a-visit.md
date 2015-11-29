title: 'Day 4: Making a Page Worth a Visit'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3061
categories:
  - Code Dive
date: 2014-06-04 10:04:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

A page instantly becomes more interesting with useful data and a bit of style. Today we’re going to stuff a bit of data into our view and rock it out a little bit with some Bootstrap style.

## Introducing Some Data

If you’ve been [following along](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/) you’ll have a controller called SimpleController with an Index method in it.&nbsp; I’m going to come back to that in a moment, but first we need a class to store our data in.&nbsp; Right-click on the Models folder in the solution and select Add –&gt; Class…, then name it Person from the dialog.&nbsp; Add FirstName, LastName, Birthdate, LikesMusic and Skills (an ICollection of string) properties, so it ends up looking like so:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> Person
{
    <span class="kwrd">public</span> <span class="kwrd">string</span> FirstName { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">string</span> LastName { get; set; }
    <span class="kwrd">public</span> DateTime BirthDate { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">bool</span> LikesMusic { get; set; }
    <span class="kwrd">public</span> ICollection&lt;<span class="kwrd">string</span>&gt; Skills { get; set; }
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
Those properties should give us some interesting data to look at.

Now, return to your Index method on the SimpleController and update the code to do the following:
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult Index()
{
    var person = <span class="kwrd">new</span> Person
    {
        FirstName = <span class="str">"Billy Jo"</span>,
        LastName = <span class="str">"McGuffery"</span>,
        BirthDate = <span class="kwrd">new</span> DateTime(1990, 6,1),
        LikesMusic = <span class="kwrd">true</span>,
        Skills = <span class="kwrd">new</span> List&lt;<span class="kwrd">string</span>&gt;() { <span class="str">"Math"</span>, <span class="str">"Science"</span>, <span class="str">"History"</span> }
    };

    <span class="kwrd">return</span> View(person);
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

We are initializing a new Person object and updating the call to View() to pass in the person object.

**_Note of Awesome_** In previous versions of the MVC Framework and Visual Studio, we used to have to build our solution to get the types available to the tooling, this is no longer the case, allowing for a little less back-and-forth and confusion over missing types in the tooling dialogs. A nice new feature!

## Scaffolding a View

Now, as nice as that [Index page was](http://jameschambers.com/2014/06/day-3-adding-a-controller-and-view/) that we created in the Simple view folder, it’s just got to go! Select it from the Solution Explorer and delete it, then return to your SimpleController code file and create a new view in a similar fashion. This time, however, when you’re creating the view, you’ll need to select the “Details” template and the “Person” class.

![image](http://jameschambers.com/wp-content/uploads/2014/06/image5.png "image")

Now when you run your application and navigate to Simple/Index, you’ll see something like the following:

![image](http://jameschambers.com/wp-content/uploads/2014/06/image6.png "image")

…which is more interesting, but it just doesn’t sing.&nbsp; We need it to look more like…

![image](http://jameschambers.com/wp-content/uploads/2014/06/image7.png "image")

Bang! Now we’re snapping!&nbsp; Let’s see how that breaks down.

## Bootstrapification

The page is missing some _zing_&nbsp; and some data. So we need to fix that. The scaffolder does its best to drop properties on the page, but it doesn’t do so well with collections (without special instructions) and tends to just dump the properties in, more or less, a list. It’s a good place to start and helps get a minimal viable product out the door, but it’s not pretty.

To get the page looking like version 2 above, there are four main components that we need to address. The first is just the setup code for the page, where you set the model type, the page title and collect some information to help render the view.
<pre class="csharpcode">@model WebApplication29.Models.Person

@{
    ViewBag.Title = <span class="str">"Index"</span>;
    var likesMusic = Model.LikesMusic ? <span class="str">"active"</span> : <span class="kwrd">null</span>;
    var notAMusicFan = !Model.LikesMusic ? <span class="str">"active"</span> : <span class="kwrd">null</span>;
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

Next is three divs, each with a slightly different purpose. The first is the page header, a place to block out some info on the page and relay context.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="page-header"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">h1</span><span class="kwrd">&gt;</span>Welcome to the Person Page <span class="kwrd">&lt;</span><span class="html">small</span><span class="kwrd">&gt;</span>Read all about @Model.FirstName<span class="kwrd">&lt;/</span><span class="html">small</span><span class="kwrd">&gt;&lt;/</span><span class="html">h1</span><span class="kwrd">&gt;</span>
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

Then we push out the primary details of the person.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">h3</span><span class="kwrd">&gt;</span>@Model.FirstName @Model.LastName<span class="kwrd">&lt;/</span><span class="html">h3</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>@Model.FirstName was born on @Model.BirthDate.ToString("D").<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="btn-group"</span> <span class="attr">data-toggle</span><span class="kwrd">="buttons"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">label</span> <span class="attr">class</span><span class="kwrd">="btn btn-success btn-sm @likesMusic"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">name</span><span class="kwrd">="options"</span> <span class="attr">id</span><span class="kwrd">="option1"</span><span class="kwrd">&gt;</span> Likes Music
            <span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">label</span> <span class="attr">class</span><span class="kwrd">="btn btn-success btn-sm @notAMusicFan"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">name</span><span class="kwrd">="options"</span> <span class="attr">id</span><span class="kwrd">="option2"</span><span class="kwrd">&gt;</span> Suffers in a Distorted Reality
            <span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
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

And finally, we loop through all the skills the person has and set them up as labels.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
        Skills:
        @foreach (var skill in Model.Skills)
        {
            <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="label label-primary"</span><span class="kwrd">&gt;</span>@skill<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
        }
    <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
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

## Up Next…

Now that we’re looking at pulling some real data into our views, it’s likely a good time to step back and get a feel for what Bootstrap is and how it can help us style our pages. Tomorrow we’ll take a look at what Bootstrap has to offer our UI.