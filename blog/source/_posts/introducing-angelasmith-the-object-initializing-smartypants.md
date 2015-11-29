title: Introducing AngelaSmith - The Object Initializing Smartypants
tags:
  - AngelaSmith
  - open source
  - seed data
  - testing
id: 141
categories:
  - Projects
date: 2012-12-24 17:09:00
---

Our tooling in the .Net space is getting better and better, as well as the ecosystem that we work in. Take for instance Windows Azure Mobile Services – when we are dealing with greenfield development having your backend services running in minutes is a Godsend and having it work over multiple devices is chocolate on the sundae.&nbsp; Your database is in place in minutes and you can easily access your data via REST.

What is something that all data-based applications require?&nbsp; Data!&nbsp; But missing from the data equation in green field development are two key parts to new app development: sample data and design time data.&nbsp; If you understand those parts already and want to be able to fire up 250 persons like this:
<pre class="csharpcode">     var people = Angie
        .Configure()
        .ListCount(250)
        .MakeList&lt;Person&gt;();</pre>
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

...then skip below to the juicy bits.&nbsp; For the rest of us, a quick primer.

## Why It's Important

For those of us building Windows Store or Windows 8 Phone apps, we have some incredible tools that make designing good looks apps super easy.

First is the MvvmLight Toolkit from [Laurent Bugnion](https://twitter.com/LBugnion) that allows you to quickly (and selectively) build out bindable view models, complete with properties, commands and, of course, your data.&nbsp; If you haven't worked with the data-binding approach, this is something you should be looking at if you use data at all in your apps.&nbsp; If you're not working with data at all in your apps, this is something that you should be looking at if you want users at all in your apps.

Second is the ability to use design time data through tools like Blend or Visual Studio 2012\. Design time data sources usually provide static methods that can supply your IDE or editor of choice sample data while you're designing your view.&nbsp; It's tough to build out a good looking UI without having actual data in there to see what things will look like.

When you put these two things together you get this great experience of seeing your app come together without having to build, deploy and run your app.

In other environments (Android, XCode) there may be similar facilities for design, but on all platforms, there's the actual execution of the app on the device, and you probably want to get some data up there.&nbsp; How do you test user-level security with only one user?&nbsp; How do you build a geolocation app presenting points of interest if you don't have any POI data?&nbsp; View a contact list with no contacts?&nbsp; What about just seeing how you app performs under load?&nbsp; Build screen shots for your app store shots?&nbsp; 

You get the idea. You need data in there.

## So You Want to Do the Data Binding Awesomeness

Great, so we agree that some measure of data is needed at some point of the app build process. You need a list of, say, 25 people, but how do you get those in there?&nbsp; This is how we used to do it:
<pre class="csharpcode">        List&lt;Person&gt; people = <span class="kwrd">new</span> List&lt;Person&gt;();
        people.Add(<span class="kwrd">new</span> Person { FirstName = <span class="str">"Angela"</span>, LastName = <span class="str">"Smith"</span>, Age = 24, PhoneNumber = <span class="str">"867-5309"</span> });
        <span class="rem">// ...do this 23 times...</span>
        people.Add(<span class="kwrd">new</span> Person { FirstName = <span class="str">"Susan"</span>, LastName = <span class="str">"Peters"</span>, Age = 22, PhoneNumber = <span class="str">"555-1234"</span> });
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

But that sucks. Big time.&nbsp; Because you have to new up every object, and to make it look somewhat interesting, you have to also touch each row (not copy and paste). So you have to sift through 25 lines of code and try to come up with some believable objects. 

## A Way With Way More Awesome

How about this instead:
<pre class="csharpcode">        List&lt;Person&gt; people = Angie.FastList&lt;Person&gt;();</pre>
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
Oh yeah. That's how AngelaSmith rolls. One line of code to get a list of 25 people (25 is the default list size).&nbsp; All the properties are filled in for you as Angie recognizes many common property names. 

You can use Angie to build out a single object:
<pre class="csharpcode">        var person = Angie.FastMake&lt;Person&gt;();</pre>
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
Angie’s not alone; she has a friend who can also help fill in a single property: <pre class="csharpcode">        <span class="kwrd">string</span> firstName = Susan.FillFirstName();</pre>
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

You can tell her how to build properties:
<pre class="csharpcode">    var blogTitle = <span class="str">"Angie"</span>;

    var post = Angie.Configure()
        .FillBy(<span class="str">"title"</span>, <span class="kwrd">delegate</span>() { <span class="kwrd">return</span> blogTitle; })
        .Make&lt;BlogPost&gt;();</pre>
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

Or, she can help you fill in properties (because she’s so smart):
<pre class="csharpcode">     var comments = Angie
        .Configure()
        .ListCount(1000)
        .FillBy(<span class="str">"CommentDate"</span>, <span class="kwrd">delegate</span>() { <span class="kwrd">return</span> Angie.MakeDate(DateRules.PastDates); })
        .MakeList&lt;BlogComment&gt;();</pre>
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

And, you can chain it all together to do some cool things.&nbsp; Here, we’re building a blog post with 5 comments, all the dates are set to a date prior today, and the comments are created using an anonymous function:
<pre class="csharpcode">  var blogpost = Angie
    .Configure()
    .FillBy(<span class="str">"CreateDate"</span>, <span class="kwrd">delegate</span>() { <span class="kwrd">return</span> Susan.FillDate(DateRules.PastDate); })
    .FillBy(<span class="str">"Comments"</span>, <span class="kwrd">delegate</span>()
    {
      <span class="kwrd">return</span> Angie
          .Set()
          .ListCount(5)
          .FillBy(<span class="str">"CommentDate"</span>, <span class="kwrd">delegate</span>() { <span class="kwrd">return</span> Susan.FillDate(DateRules.PastDate); })
          .MakeList&lt;BlogComment&gt;();
    })
    .Make&lt;BlogPost&gt;();</pre>
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

At the tail of that, we’re calling “Make” to get a single instance of BlogPost, but we could just as easily call “MakeList” to get a list of entities.

## So Get Going!

To install AngelaSmith, use the Package Manager Console:
<pre class="csharpcode">    install-package AngelaSmith</pre>
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

For more information, check out the [AngelaSmith project on GitHub](https://github.com/MisterJames/AngelaSmith). Of course, you're welcome to fork and contribute!

Stay tuned for great examples on how to seed data in Windows Azure Mobile Services, Entity Framework and more.