title: 'Day 10: HtmlHelper Extension Methods'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3381
categories:
  - Code Dive
date: 2014-06-10 10:13:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

As we extracted our Person template we stubbed in a placeholder for a person’s avatar. Rather than creating our own system for uploading, resizing and storing the images, we’ll use a commonly used service on the internet called Gravatar to display one that many users might already have.

## A Gravatar Extension Method

The format for Gravatar images is as follows:
 > [http://www.gravatar.com/avatar/MD5HASH?options](http://www.gravatar.com/avatar/MD5HASH?options) 

The MD5 hash is computed based on their email address, and there are a few options worth noting. Below are the querystring parameters we’ll be using to generate our image.

*   **Default Image**: the image or type of image to use or generate if there isn’t a Gravatar for the specified email address.  <li>**Size**: the size of the image to be returned, always a square.  <li>**Rating**: users self-specify their rating and can use different avatars for different audiences. 

We’ll represent those options in an class that we’ll use as a parameter.&nbsp; Create a Helpers folder, then create a class called GravatarOptions in it.
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> GravatarOptions
{
    <span class="kwrd">public</span> <span class="kwrd">string</span> DefaultImageType { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">string</span> RatingLevel { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">int</span> Size { get; set; }

    <span class="kwrd">public</span> <span class="kwrd">class</span> DefaultImage
    {
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Default = <span class="str">""</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Http404 = <span class="str">"404"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> MysteryMan = <span class="str">"mm"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Identicon = <span class="str">"identicon"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> MonsterId = <span class="str">"monsterid"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Wavatar = <span class="str">"wavatar"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Retro = <span class="str">"retro"</span>;
    }

    <span class="kwrd">public</span> <span class="kwrd">class</span> Rating
    {
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> G = <span class="str">"g"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> PG = <span class="str">"pg"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> R = <span class="str">"r"</span>;
        <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> X = <span class="str">"x"</span>;
    }

    <span class="kwrd">internal</span> <span class="kwrd">static</span> GravatarOptions GetDefaults()
    {
        <span class="kwrd">return</span> <span class="kwrd">new</span> GravatarOptions
        {
            DefaultImageType = GravatarOptions.DefaultImage.Retro,
            Size = 150,
            RatingLevel = GravatarOptions.Rating.G
        };
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

And finally, add a class called GravatarHelper, then add the following code:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">static</span> <span class="kwrd">class</span> GravatarHelper
{
    <span class="kwrd">public</span> <span class="kwrd">static</span> HtmlString GravatarImage(<span class="kwrd">this</span> HtmlHelper htmlHelper, <span class="kwrd">string</span> emailAddress, GravatarOptions options = <span class="kwrd">null</span>)
    {
        <span class="kwrd">if</span> (options == <span class="kwrd">null</span>)
            options = GravatarOptions.GetDefaults();

        var imgTag = <span class="kwrd">new</span> TagBuilder(<span class="str">"img"</span>);

        emailAddress = <span class="kwrd">string</span>.IsNullOrEmpty(emailAddress) ? <span class="kwrd">string</span>.Empty : emailAddress.Trim().ToLower();

        imgTag.Attributes.Add(<span class="str">"src"</span>,
            <span class="kwrd">string</span>.Format(<span class="str">"http://www.gravatar.com/avatar/{0}?s={1}{2}{3}"</span>,
                GetMd5Hash(emailAddress),
                options.Size,
                <span class="str">"&amp;d="</span> + options.DefaultImageType,
                <span class="str">"&amp;r="</span> + options.RatingLevel
                )
            );

        <span class="kwrd">return</span> <span class="kwrd">new</span> HtmlString(imgTag.ToString(TagRenderMode.SelfClosing));
    }

    <span class="rem">// Source: http://msdn.microsoft.com/en-us/library/system.security.cryptography.md5.aspx</span>
    <span class="kwrd">private</span> <span class="kwrd">static</span> <span class="kwrd">string</span> GetMd5Hash(<span class="kwrd">string</span> input)
    {
        <span class="kwrd">byte</span>[] data = MD5.Create().ComputeHash(Encoding.UTF8.GetBytes(input));
        var sBuilder = <span class="kwrd">new</span> StringBuilder();
        <span class="kwrd">for</span> (<span class="kwrd">int</span> i = 0; i &lt; data.Length; i++)
        {
            sBuilder.Append(data[i].ToString(<span class="str">"x2"</span>));
        }
        <span class="kwrd">return</span> sBuilder.ToString();
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

Extension methods accept the type you are extending as the first parameter and operate on the instance of the object, though they are defined as a static. This may help understand why you must pass the first parameter as the type you want to extend with the _this_ modifier.&nbsp; You can then optionally any additional parameters that you will need to work with. For us, we’re just accepting the email address and a GravatarOptions instance.

In our helper, we create a tag builder for images, then build the URL based on the options the user has provided. If they haven’t provided the parameter, we simply load a set of defaults from our options class.

_**Note**: This is an overly-simplified version of [Andrew Freemantle’s](https://github.com/AndrewFreemantle) work on his [Gravatar-HtmlHelper](https://github.com/AndrewFreemantle/Gravatar-HtmlHelper) project on GitHub. Please visit his project for a more complete implementation._

## Using the Gravatar in our Person Template

Our HTML helper will work like any other HTML helper, but we need to let the MVC Framework know where to find it. To do so, we’ll have to go and add the namespace to our Web.Config in our Views folder (located at Views\Web.Config). We could also add a using statement to each page where we want to use our helper, but adding it to the web config file makes it globally available throughout our views. Add the following to the Razor namespaces section in that file:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">add</span> <span class="attr">namespace</span><span class="kwrd">="SimpleSite.Helpers"</span><span class="kwrd">/&gt;</span></pre>

We’ll need an email address, so add the following property in Person.cs.
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">string</span> EmailAddress { get; set; }</pre>
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

Next, jump back into your SimpleController class and update the instantiation of the person object so that they have a value in there:
<pre class="csharpcode">var person = <span class="kwrd">new</span> Person
{
    FirstName = <span class="str">"Billy Jo"</span>,
    LastName = <span class="str">"McGuffery"</span>,
    BirthDate = <span class="kwrd">new</span> DateTime(1990, 6,1),
    LikesMusic = <span class="kwrd">true</span>,
    EmailAddress = <span class="str">"Bill@jo.com"</span>,
    Skills = <span class="kwrd">new</span> List&lt;<span class="kwrd">string</span>&gt;() { <span class="str">"Math"</span>, <span class="str">"Science"</span>, <span class="str">"History"</span> }
};</pre>
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

Now, update your Person.cshtml template by replacing the image tag with the following:
<pre class="csharpcode">@Html.GravatarImage(Model.EmailAddress)</pre>
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

Then, pop into your browser to see the fruits of your effort!

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb3.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image18.png)

The gravatar image will be updated for any email address that you put in. You can try playing with the different defaults, your own email address or other options. 

_**Another note:** I wouldn’t actually advocate creating a full implementation of an HtmlHelper method for Gravatars. The point of this exercise was to walk through a simple implementation, but there are much more complete helpers available on NuGet, ready to be deployed to your app. If you’d like to work on one of them, they are pretty much all open source, so feel free to contribute!_

## Next Steps

Great, now we have a basic extension method to make outputting a Gravatar much more simply, but we’re going to need to address our Index view. It’s typically a list of entities, with a link to a details view, not just a single item. Tomorrow, we’ll tackle getting that list set up and move the single view version to it’s own action and view.