title: Resolving Build Errors When Targeting Mulitple Framework Versions
tags:
  - 'c#'
id: 2682
categories:
  - Code Dive
date: 2014-03-01 17:41:48
---

Here’s a tip that I hope can help some other folks when working on a solution that targets multiple versions of the .Net Framework.

As a developer, I tend to have a short memory and flush it often. When I start using framework features, I let myself easily move on, mentally, from the time when said features didn’t exist. 2010 is _sooo_ last year. Or four years ago, but who’s keeping count.

This morning, I started getting the following error while working on [Glimpse](http://github.com/glimpse/glimpse), where the primary project is authored in .Net 4.5:
 > System.Enum does not contain a definition for ‘TryParse’. 

![image](https://jcblogimages.blob.core.windows.net/img/2014/03/image.png "image")

A quick check on MSDN shows that [System.Enum](http://msdn.microsoft.com/en-us/library/dd783499(v=vs.110).aspx) _does indeed_ contain a definition for TryParse, but only in 4.0 and higher.

&nbsp;[![image](https://jcblogimages.blob.core.windows.net/img/2014/03/image_thumb.png "image")](https://jcblogimages.blob.core.windows.net/img/2014/03/image1.png)

If you peek back at the Error List screen cap, you’ll see the hint to what was going on in the “Project” column. Namely, one of the projects in the Glimpse solution used for backwards compatibility targets an older version of the .Net Framework.

So, this is actually pretty easy to resolve, and I have two obvious choices:

1.  I can test to see if the NET35 compilation symbol is defined and write two copies of the code, one with, one without the use of TryParse, or,
2.  Just use the cross-framework supported approach from way back in the day (2010), where we would wrap up the Enum.Parse call in a try-catch block. 

For brevity of code, I chose #2\. 
<pre class="csharpcode">    <span class="kwrd">try</span>
    {
        order = (ScriptOrder)Enum.Parse(<span class="kwrd">typeof</span>(ScriptOrder), orderName);
    }
    <span class="kwrd">catch</span> (ArgumentException)
    {
        <span class="kwrd">return</span> <span class="kwrd">new</span> StatusCodeResourceResult(404, <span class="kwrd">string</span>.Format(<span class="str">"Could not resolve ScriptOrder for value provided '{0}'."</span>, orderName));
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

Should Glimpse drop support for .Net 3.5 down the road, this would be an easy pull request to update and make use of the new(ish) TryParse method.

Filed under the category of “things to keep fresh in your mind when working on open source”.

For more on multi-targeted solutions, you can check out [this read on MSDN](http://msdn.microsoft.com/en-us/library/vstudio/bb398197(v=vs.120).aspx).