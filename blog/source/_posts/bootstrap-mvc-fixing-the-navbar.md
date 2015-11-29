title: Bootstrap + Mvc - Fixing the Navbar
tags:
  - Asp.Net MVC
  - Bootstrap
id: 521
categories:
  - Code Dive
date: 2012-10-12 17:35:00
---

I am continuing my series on using Twitter's Bootstrap CSS and JavaScript library with the Asp.Net Mvc 4 Framework. Here's the [rest of the series](http://oldblog.jameschambers.com/blog/bootstrap-asp.net-mvc-quickhits-installing-bootstrap), and today's instalment of the video is here:
<iframe height="315" src="http://www.youtube.com/embed/uA7Z8WfD3bU" frameborder="0" width="560" allowfullscreen></iframe> 

User "arun" wrote in on [my previous post](http://oldblog.jameschambers.com/blog/bootstrapping-mvc---the-completed-template), asking about a problem with the rendering of the navbar when the responsive design elements kicked in. I fired up the project to see what was amiss.&nbsp; What I found when I made the browser more narrow was this:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Bootstrap--Mvc---Fixing-the-Navbar_BBD9/image_thumb_2.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Bootstrap--Mvc---Fixing-the-Navbar_BBD9/image_6.png)

What's with that white space?&nbsp; Here's what we _really _want:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Bootstrap--Mvc---Fixing-the-Navbar_BBD9/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Bootstrap--Mvc---Fixing-the-Navbar_BBD9/image_8.png)

Yay! No whitespace! How did we make it happen? Turns out it's quite easy, just make your layout header look like this:
<pre class="csharpcode">    <span class="kwrd">&lt;</span><span class="html">style</span><span class="kwrd">&gt;</span>
        body { padding-top: 60px; /* 60px to make the container go all the way to the bottom of the topbar */ }
        .aligntobottom { bottom:0; position:absolute; }
    <span class="kwrd">&lt;/</span><span class="html">style</span><span class="kwrd">&gt;</span>

    @Scripts.Render("~/bundles/modernizr")
    @Styles.Render("~/Content/Bootstrap")</pre>
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

Notice that we're moving the Bootstrap library _below_ the body padding style so that we can get around a CSS conflict.

**Don't forget to watch the video above for a gem in there on getting the authentication bits going!**

# What's Next?

For next steps, here's some things to try out:

*   Check out the other instalments [in this series](http://oldblog.jameschambers.com/blog/bootstrap-asp.net-mvc-quickhits-installing-bootstrap)<li>Download the RTM version of the [Mvc 4 Framework](http://www.asp.net/mvc) and build the site out with posts from the series<li>Follow me [@CanadianJames](http://www.twitter.com/CanadianJames) to find out when new articles/videos are posted in this series ![Smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Bootstrap--Mvc---Fixing-the-Navbar_BBD9/wlEmoticon-smile_2.png)

&nbsp;

Catch you on the next post, thanks all for your comments and questions!