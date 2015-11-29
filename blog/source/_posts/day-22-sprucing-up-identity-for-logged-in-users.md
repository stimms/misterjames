title: 'Day 22: Sprucing up Identity for Logged In Users'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4151
categories:
  - Code Dive
date: 2014-06-28 03:37:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Typically you wouldn’t want the entirety of the internet adding and editing records at will without some form of authentication and authorization so that you can keep track of who’s editing your data, or so that your users can keep track of their own.

So, let’s continue with our project and build out some identity capabilities.

## Okay, I’m Kidding. Just Press F5!

Great work, you’re all done!

As most folks are well aware, the default templates for Asp.Net applications now ship with a sample account administration implementation (reasonably good ones, at that). Users are able to register and log in using pre-built models, controllers and views. You can easily extend the template with 3rd party login capabilities, allowing folks with Microsoft, Facebook or Google accounts (and, and, and…) to log into your site as well. We’ve now got substantial improvements to identity management overall, with things like two-factor authentication, reductions in the leaky abstractions we’ve lived with, better testability, more flexibility in storage options, and more.

If you’re not familiar with these bits, it’s worth having a look, **but this is a topic already well covered**. Believe me, these are worth the reads:

*   The Asp.Net Identity [web site](http://www.asp.net/identity)
*   A [background article on Identity](http://www.asp.net/identity/overview/getting-started/introduction-to-aspnet-identity) by the Asp.Net team
*   Two-Factor [authentication](http://www.asp.net/identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity) with SMS
*   Using [third-party authentication providers](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on) on your site (Facebook, Twitter, Google, LinkedIn) 

But we’re here to do Bootstrappy things, right? So let’s spruce up that top bar a little for our logged in users with some MVC bits we build (HTML helpers) and some Bootstrap bits (image classes) that will take our site up a level.

## Bootstrap Image Classes

The CSS library gives us a few easy-to-use helpers to make our images look consistent. Here’s a sample from the [Bootstrap site](http://getbootstrap.com/css/#images):

![image](http://jameschambers.com/wp-content/uploads/2014/06/image38.png "image")

The classes are as follows:

*   img-rounded – provides rounded corners to your rectangular images
*   img-circle – turns any image into a circle
*   img-thumbnail – makes your image look like a little Polaroid of itself 

## Making Members Feel Welcome

Today we’re just going to add a little touch to the navbar that our logged in users will see, keying in off of the default implementation of the Identity providers.&nbsp; We’ll head in that direction by extending our HtmlHelper that generates Gravatar images, as we need to add a property for a CSS class to give us a nice round face on the page.&nbsp; But first, we’ll have to add the following line of code to the GravatarOptions class:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">string</span> CssClass { get; set; }</pre>

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
We’ll need to add that to the tag, so let’s revist the GravatarImage, likely located in the Helpers folder, to check for a value and add it to the tag if present. The full method should now look like this:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">static</span> HtmlString GravatarImage(<span class="kwrd">this</span> HtmlHelper htmlHelper, <span class="kwrd">string</span> emailAddress, GravatarOptions options = <span class="kwrd">null</span>)
{
    <span class="kwrd">if</span> (options == <span class="kwrd">null</span>)
        options = GravatarOptions.GetDefaults();

    var imgTag = <span class="kwrd">new</span> TagBuilder(<span class="str">"img"</span>);

    emailAddress = <span class="kwrd">string</span>.IsNullOrEmpty(emailAddress) ? <span class="kwrd">string</span>.Empty : emailAddress.Trim().ToLower();

    <span class="rem">// &lt;-- adding support for CSS</span>
    <span class="kwrd">if</span> (!<span class="kwrd">string</span>.IsNullOrEmpty(options.CssClass))
    {
        imgTag.AddCssClass(options.CssClass);
    }
    <span class="rem">// adding support for CSS  --&gt;</span>

    imgTag.Attributes.Add(<span class="str">"src"</span>,
        <span class="kwrd">string</span>.Format(<span class="str">"http://www.gravatar.com/avatar/{0}?s={1}{2}{3}"</span>,
            GetMd5Hash(emailAddress),
            options.Size,
            <span class="str">"&amp;d="</span> + options.DefaultImageType,
            <span class="str">"&amp;r="</span> + options.RatingLevel
            )
        );

    <span class="kwrd">return</span> <span class="kwrd">new</span> HtmlString(imgTag.ToString(TagRenderMode.SelfClosing));
}</pre>

Unfortunately there isn’t quite the exact Bootstrap class we need to make our image place and size well in the navbar, so we’ll need to open up our bootstrap.custom.css file and add the following structural class:
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

<pre class="csharpcode">.navbar-image {
  float: left;
  padding: 10px 5px;
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

Finally, we’ll pop over to our _LoginPartial.cshtml (in Views\Shared) and modify the block of code displayed for logged in users.&nbsp; Because I’m reusing the user’s name a couple of times (which is also their email address) I’m storing it in a variable first, and using that throughout the block. I also add a DIV as a container for our image, using the class that we just created. Last, I make a call to our GravatarImage helper, passing in the username (email!), an appropriate size for the toolbar (30px), and the Bootstrap class that gives us the shape we’re looking for (img-circle).
<pre class="csharpcode">var username = User.Identity.GetUserName();
using (Html.BeginForm("LogOff", "Account", FormMethod.Post, new { id = "logoutForm", @class = "navbar-right" }))
{

@Html.AntiForgeryToken()

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="navbar-image"</span><span class="kwrd">&gt;</span>
    @Html.GravatarImage(username, new GravatarOptions { Size = 30, CssClass = "img-circle" })
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>

<span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="nav navbar-nav navbar-right"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>
        @Html.ActionLink("Hello " + username + "!", "Manage", "Account", routeValues: null, htmlAttributes: new { title = "Manage" })
    <span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="javascript:document.getElementById('logoutForm').submit()"</span><span class="kwrd">&gt;</span>Log off<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>
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

And now we’re rockin’ head shots in our navbar!

![image](http://jameschambers.com/wp-content/uploads/2014/06/image39.png "image")

## Next Steps

Perhaps you’ve modified your user profiles so that the username doesn’t have to be an email address. You could easily modify this example to read from a different property and generate the same type of effect.

What is sparkle without shine? Let’s give our users the ability to choose their own look-and-feel.&nbsp; I mean, it worked for GeoCities, right?