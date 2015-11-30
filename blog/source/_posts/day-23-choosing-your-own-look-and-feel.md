title: 'Day 23: Choosing Your Own Look-And-Feel'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4221
categories:
  - Code Dive
date: 2014-06-29 12:28:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Here’s the thing: everybody’s site is going to look the same if everybody’s site uses Bootstrap, right? Well, if we did that to users, we’d sure be missing the point as developers, and for two key reasons: 1) With the LESS and SASS source it’s easy to customize, and 2) Because it’s easy to customize, there’s already people creating all kinds of alternate themes for Bootstrap.

So, we aren’t headed down a slippery slope here, we just need to make a little effort to give our users some unique looking sites.&nbsp; Other posts will show you how to replace the Bootstrap theme, but this one will show you how to let your users choose from a list you’ve pre-built.

## Download a Free Substitute

There are some great, free alternatives located at [Bootswatch.com](http://bootswatch.com/). So, start there and pick one or two to download, I chose Amelia and Darkly. We need to create a folder structure to organize our themes, and move the CSS into those folders. Mine ended up working like this:

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image40.png "image")

Note that I also pushed a copy of the stock CSS for bootstrap into this structure. This allows us to simplify our code for theme switching, allowing users to pick the base theme if they like.

**Shameless plug**: If you’re looking for professionally designed themes to replace your palette, you can help out this blogger (me!) by purchasing one over at [{wrap}bootstrap](https://wrapbootstrap.com/?ref=BootstrapOnMvc). They have a selection of great looking Bootstrap themes. A very affordable alternative to taking the time to create your own theme.

## Creating a Helper Class

Next, we create a class called Bootstrap.cs (I put mine in \Helpers) so that we can programmatically work with the themes. This class is responsible for presenting the list of supported themes and resolving the path to them when we try to load the bundles.
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> Bootstrap
{
    <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> BundleBase = <span class="str">"~/Content/css/"</span>;

    <span class="kwrd">public</span> <span class="kwrd">class</span> Theme
    {
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Stock = <span class="str">"Stock"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Amelia = <span class="str">"Amelia"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Darkly = <span class="str">"Darkly"</span>;
    }

    <span class="kwrd">public</span> <span class="kwrd">static</span> HashSet&lt;<span class="kwrd">string</span>&gt; Themes = <span class="kwrd">new</span> HashSet&lt;<span class="kwrd">string</span>&gt;
    {
        Theme.Stock,
        Theme.Amelia,
        Theme.Darkly
    };

    <span class="kwrd">public</span> <span class="kwrd">static</span> <span class="kwrd">string</span> Bundle(<span class="kwrd">string</span> themename)
    {
        <span class="kwrd">return</span> BundleBase + themename;
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

This is a simple class, but it prevents us from duplicating code all over the place or unnecessary spelling mistakes.

The other nice thing is that if you choose to add another theme to your project, you will just have to modify this one file and the rest will fall into place.&nbsp; For that to happen, we’ll need to modify our startup to generate all the appropriate bundles.

## Updating Our Startup Bits

Head into BundleConfig.cs (inside of \App_Startup) and replace the code that creates the Bootstrap bundle with the following:
<pre class="csharpcode"><span class="kwrd">foreach</span> (var theme <span class="kwrd">in</span> Bootstrap.Themes)
{
    var stylePath = <span class="kwrd">string</span>.Format(<span class="str">"~/Content/Themes/{0}/bootstrap.css"</span>, theme);

    bundles.Add(<span class="kwrd">new</span> StyleBundle(Bootstrap.Bundle(theme)).Include(
                stylePath,
                <span class="str">"~/Content/bootstrap.custom.css"</span>,
                <span class="str">"~/Content/site.css"</span>));
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

We’re simply looping over that handle collection we created so that we could generate a bundle for every installed theme.&nbsp; We always want all the themes – startup is only run at as the application starts, so we need them all there – as different users may wish to select different themes.

## Making Our Layout “Themeable”

What we really need to do here is just a quick update to figure out the user’s current theme, and then figure out what the correct bundle to use is.
<pre class="csharpcode">@{
    var theme = Session[<span class="str">"CssTheme"</span>] <span class="kwrd">as</span> <span class="kwrd">string</span> ?? Bootstrap.Theme.Stock;
}
@Styles.Render(Bootstrap.Bundle(theme))</pre>
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

I’m using Session for now, but I’ll explain why in a bit that is bad idea. ![Smile](https://jcblogimages.blob.core.windows.net/img/2014/06/wlEmoticon-smile3.png)

Note that, at this point, you could set that theme default – right now set to Bootstrap.Theme.Stock – to whichever theme you like and run your app. The mechanics of building the bundle and the class to resolve it are all in place.

## Letting the User Choose a Theme

Once again we’re going to revisit the _LoginPartial.cshtml file (in Views\Shared). In this round, we’re going to update the text that shows the logged in user’s email address (which, by default, is also their username). The LI for the username is now going to be a dropdown box in the navbar.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">li</span> <span class="attr">class</span><span class="kwrd">="dropdown"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span> <span class="attr">class</span><span class="kwrd">="dropdown-toggle"</span> <span class="attr">data-toggle</span><span class="kwrd">="dropdown"</span><span class="kwrd">&gt;</span>Hello @username! <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="caret"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;&lt;/</span><span class="html">a</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="dropdown-menu"</span> <span class="attr">role</span><span class="kwrd">="menu"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>
            @Html.ActionLink("Manage Account", "Manage", "Account", routeValues: null, htmlAttributes: new { title = "Manage" })
        <span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span> <span class="attr">class</span><span class="kwrd">="divider"</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        @foreach (var theme in Bootstrap.Themes)
        {
            <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="@Url.Action("</span><span class="attr">ChangeTheme</span><span class="kwrd">", "</span><span class="attr">Profile</span><span class="kwrd">", new { themename = theme})"</span><span class="kwrd">&gt;</span>@theme<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        }
    <span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
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
I’ve taken that LI that was there – all it had was the user name, which was a link to Manage Account – and replaced it will all of the above code. We put in a divider and iterate over the list of known themes.&nbsp; Each will be a link to a “ChangeTheme” action on the “Profile” controller.

## Adding our New Profile Controller

Throw ProfileController.cs in your \Controllers directory with the following, lone action in the class:
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult ChangeTheme(<span class="kwrd">string</span> themename)
{
    Session[<span class="str">"CssTheme"</span>] = themename;
    <span class="kwrd">if</span> (Request.UrlReferrer != <span class="kwrd">null</span>)
    {
        var returnUrl = Request.UrlReferrer.ToString();
        <span class="kwrd">return</span> <span class="kwrd">new</span> RedirectResult(returnUrl);
    }
    <span class="kwrd">return</span> RedirectToAction(<span class="str">"Index"</span>, <span class="str">"Home"</span>);
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

If we have a referring URL we can push the user back to the same page, otherwise we ship them home. The preference of theme is set in the Session.

And there you have it: users can now pick their own theme on your site:

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image41.png "image")

## Next Steps

This data doesn’t really belong in the user’s session…whenever the app pool is recycled or the website or IIS is restarted or their session expires they will lose this choice. Even worse, session and Identity aren’t on the same lifecycle, so when they log out the session persists and they’ll still see the theme they chose when they logged in.

So, where should it be stored? Tune in next time as we answer this question and more. ![Smile with tongue out](https://jcblogimages.blob.core.windows.net/img/2014/06/wlEmoticon-smilewithtongueout.png)

Happy coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2014/06/wlEmoticon-smile3.png)
