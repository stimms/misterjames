title: Adding Some (Font) Awesome to MVC and Bootstrap
tags:
  - Asp.Net MVC
  - Bootstrap
  - FontAwesome
id: 4951
categories:
  - Code Dive
date: 2014-08-24 22:19:26
---

While Bootstrap includes many great icons from **Glyphicons** baked right in, I’m often left looking for _That One Missing Graphic_ when I try to put a project together. In this post we’ll look at the open source **Font Awesome** collection of images that serve as a wonderful addition to your site in the form of a font and CSS toolkit.

_[See more tips, tricks and tutorials like this in my book, Bootstrapping MVC.](http://jameschambers.com/bootstrapping-mvc/)_

**Just a quick note**: If you already know what Font Awesome is and just want my advice on how to add it to your project via NuGet, skip to the end of the post. If you want more background on what Font Awesome is and why you might want to use it, grab a coffee and have a read!

## A Bit About Font Awesome

![A Picture of a Heart Icon](https://jcblogimages.blob.core.windows.net/img/2014/08/image.png "Heart")Font Awesome is a font and CSS toolkit that was created by Dave Gandy, originally designed to be a compliment to the Bootstrap library. There is no JavaScript component to it and it does not add any new behaviors to your site, but it does have some great-looking, scalable icons that you’ll likely love.&nbsp; See what I did there, with the heart? ![Winking smile](https://jcblogimages.blob.core.windows.net/img/2014/08/wlEmoticon-winkingsmile.png)

There are two core elements to using the library: the font files themselves and the CSS classes that wire things up for use on your site. With the library in place, you can get the above heart icon to appear simply by adding the following syntax:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">i</span> <span class="attr">class</span><span class="kwrd">="fa fa-heart"</span><span class="kwrd">&gt;&lt;/</span><span class="html">i</span><span class="kwrd">&gt;</span></pre>

Because it’s implemented as a scalable vector-based font, rather than raster images, the icons look great on any screen type and at any size. You get the base styling, margins, sizing and the like by attributing the &lt;I&gt; element with the fa class, then select your icon by name. There are CSS classes for creating variant sizes (twice as large):
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">i</span> <span class="attr">class</span><span class="kwrd">="fa fa-camera-retro fa-2x"</span><span class="kwrd">&gt;&lt;/</span><span class="html">i</span><span class="kwrd">&gt;</span></pre>
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

…adding spinner effects (here, a spinning cog):
<pre class="csharpcode">&lt;i <span class="kwrd">class</span>=<span class="str">"fa fa-cog fa-spin"</span>&gt;&lt;/i&gt;</pre>
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

…or stacking images together to make composite icons, like a flag on a circle:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="fa-stack fa-lg"</span><span class="kwrd">&gt;</span>
  <span class="kwrd">&lt;</span><span class="html">i</span> <span class="attr">class</span><span class="kwrd">="fa fa-circle fa-stack-2x"</span><span class="kwrd">&gt;&lt;/</span><span class="html">i</span><span class="kwrd">&gt;</span>
  <span class="kwrd">&lt;</span><span class="html">i</span> <span class="attr">class</span><span class="kwrd">="fa fa-flag fa-stack-1x fa-inverse"</span><span class="kwrd">&gt;&lt;/</span><span class="html">i</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span></pre>
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

…as well as other options, like rotating and flipping, adding borders and pulling the icons to the left or right. You can find the complete source on the [Font Awesome examples page](http://fortawesome.github.io/Font-Awesome/examples/).

While these display classes are all built to work with Bootstrap natively, the CSS for Font Awesome also includes everything it needs class-wise (such as pull-left) to work independently of Bootstrap.

The icons tend to be aptly named and terse, so you’re not typing a metric tonne of code. I’m using Visual Studio 2013 and working with the library is a breeze because of the automatic syntax completion offered through IntelliSense.

![HTML Code: &lt;i class=&quot;fa fa-heart&quot;&gt;&lt;/i&gt;](https://jcblogimages.blob.core.windows.net/img/2014/08/image1.png "Intellisense sample")

If you download the ZIP from the [Font Awesome](http://fontawesome.io) site, you’ll get the CSS (and minified version), the font files for multi-platform and multi-browser support as well as the LESS and SCSS files to customize things should you so need.

## A Note on Accessibility

Though the icons and images of Font Awesome are indeed implemented as a font, it’s not like wingdings or anything where a letter becomes repurposed. The author elected to use the standards-based Private Use Area mapping from Unicode, meaning screen readers for accessibility will ignore the characters of the font and not attempt to read random characters off.

## The Naked “Installation”

From the docs on the site, the easiest way to start using Font Awesome is just to use the CDN version of the toolkit.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">link</span> <span class="attr">href</span><span class="kwrd">="//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css"</span> <span class="attr">rel</span><span class="kwrd">="stylesheet"</span><span class="kwrd">&gt;</span></pre>
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

Using the CDN version may help improve load times, especially when your users are spread out over multiple geographic regions. Not all corporate policies allow for public CDN use for in-house projects, however, so you still have the option to download and use Font Awesome as an asset in your deployment.

![Solution structure for typical installation of Font Awesome](https://jcblogimages.blob.core.windows.net/img/2014/08/image2.png "Solution structure for typical installation of Font Awesome")If you do download the ZIP, you’ll need to execute a couple of familiar and straightforward tasks to use the toolkit:

*   Copy the CSS and Fonts folder into your project, likely under the Content folder
*   Add a new bundle to your site and include the CSS in your _Layout (or modify an existing, in-use bundle to include the CSS).

The only thing to watch for is that you’re going to need to honor the relative path structure of the CSS and Fonts to avoid any rendering problems. The CSS assumes the fonts are in a folder called Fonts at the same level in the path.

## Using a NuGet Package

There’s always an easier way to do it, and usually the way to do that with .NET is going to involve a little bit of NuGet.

I found no fewer than half a dozen packages on NuGet for Font Awesome. While they all seem to list Mr. Gandy as the author, none of them appear to be officially part of the project. So, for better or for worse, there’s no officially sanctioned project here. Just the same, the packages are there if you want to use them and I have my opinions on them. ![Smile](https://jcblogimages.blob.core.windows.net/img/2014/08/wlEmoticon-smile.png)

The package I recommend is [FontAwesome.MVC](https://www.nuget.org/packages/FontAwesome.MVC/) by JiveCode aka [JustLikeIcarus](https://github.com/JustLikeIcarus). This package takes a dependency on his Font Awesome package containing the CSS and fonts, then it adds a class (FontAwesomeBundleConfig) that registers a style bundle for you. 

All that’s left to do is to modify your _Layout, adding the following line of code to your HEAD:
<pre class="csharpcode">@Styles.Render(<span class="str">"~/Content/fontawesome"</span>)</pre>
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

There are other packages that use Font Awesome and try to augment MVC giving your Razor helper methods, but I’ve got mixed feelings on this approach. A Razor helper that requires me to type more code and allows me to know less about the CSS I’m using doesn’t feel quite right. With IntelliSense support for CSS in VS, I’m not sure I see the need for Razor helpers, and I really like learning the classes I’m using. 

## Next Steps

With a groovy new set of icons now available to my site, my next post will be on leveraging those to create something interesting with the MVC Framework.

_Like this post? You can [see more tips, tricks and tutorials ](http://jameschambers.com/bootstrapping-mvc/)
__like this in my book, Bootstrapping MVC._

Happy coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2014/08/wlEmoticon-smile.png)
