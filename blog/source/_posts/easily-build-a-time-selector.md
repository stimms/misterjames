title: Easily Build A Time Selector
tags:
  - Linq
id: 1861
categories:
  - Code Dive
date: 2013-03-13 01:40:05
---

I’m working on a project where I need the user to select a time in 24 hour format.&nbsp; It would have taken me a couple of minutes to punch out the lines to build a select option for each of the items that were to appear in the drop down list, but I elected to use the Enumerable.Range method instead and build the list dynamically.

[![image](https://jcblogimages.blob.core.windows.net/img/2013/03/image_thumb.png "image")](https://jcblogimages.blob.core.windows.net/img/2013/03/image.png)

Why approach it this way?&nbsp; Well, if the requirements change, for example, and I need to change that to every 10 minutes instead of every 30 minutes, I don’t need to go and write another 96 lines of code. All I have to do is change the call the select projection where I compute the minutes (right now at the top and bottom of the hour).

## A Picture is Worth Zero Lines of Code. Apparently.

So…I’m getting flack either way…either I post a code-image or I post a Gist. With a Gist, RSS readers can’t render the code. If I post an image, _no one_ can copy and paste. So, there’s the image for you in your RSS readers, and here’s the Gist for those copy and paste aficionados.
<script src="https://gist.github.com/MisterJames/5148675.js"></script> 

## Breaking Down the Code

This is actually more verbose than it needs to be but it makes it a little easier to walk through it.&nbsp; The first two statements are simply capturing some integer values to represent the hours and minutes that I want to render.
<pre class="csharpcode">

    IEnumerable&lt;<span class="kwrd">int</span>&gt; hours =
        Enumerable.Range(0, 24);

    IEnumerable&lt;<span class="kwrd">int</span>&gt; minutes =
        Enumerable.Range(0, 2)

          .Select(x =&gt; x * 30);
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

The call to Enumerable.Range takes two parameters: the first is where to start counting and the second is how many steps to take.&nbsp; In the second statement I am also projecting the minutes out by multiplying them by 30\. If I wanted, instead to have the list include 6 intervals on the 10, I would change the code to the following:
<pre class="csharpcode">    IEnumerable&lt;<span class="kwrd">int</span>&gt; minutes =
        Enumerable.Range(0, 6)
        .Select(x =&gt; x * 10);</pre>

All that’s left to do is build our strings.&nbsp; We’re going to loop over our hours, and then format those intro a string along with each of the minutes that we’ve projected into our list:
<pre class="csharpcode">    List&lt;<span class="kwrd">string</span>&gt; timeOptions = <span class="kwrd">new</span> List&lt;<span class="kwrd">string</span>&gt;();

    <span class="kwrd">foreach</span> (var hour <span class="kwrd">in</span> hours)
    {
        <span class="kwrd">foreach</span> (var minute <span class="kwrd">in</span> minutes)
        {
            timeOptions.Add(<span class="kwrd">string</span>.Format(<span class="str">"{0}:{1:00}"</span>, hour, minute));
        }
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
That’s it! The timeOptions variable now contains a list of any of the valid options we want our user to select from. You can now easily use this collection to build a drop down list for whatever client you’re building for. 

&nbsp;

Want the condensed version? Check here: [Build a list of valid times](https://gist.github.com/MisterJames/5148715)