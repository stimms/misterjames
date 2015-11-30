title: System.Web.WebPages Aren’t the HtmlHelpers You’re Looking For…
tags:
  - Asp.Net MVC
  - Extension Methods
  - HtmlHelper
  - Razor
id: 1791
categories:
  - Uncategorized
date: 2013-02-12 17:47:29
---

Had a bit of a frustrating morning, one of those times when your tools work against you by helping you out but cost you 35 minutes and an extra cup of coffee.&nbsp; And maybe a sanity check or two.&nbsp; The goal was to create a _very simple_ HtmlHelper extension method.&nbsp; I kept getting the error:
 > 'System.Web.Mvc.HtmlHelper&lt;dynamic&gt;' does not contain a definition for 'MethodName' and the best extension method overload 'Namespace.MethodName(System.Web.WebPages.Html.HtmlHelper)' has some invalid arguments 

**_TL;DR_** – Check your using statements.&nbsp; If you’re sharp, the error’s quite indicative of what’s going on.

Here’s another hint as to why this went south on me:

![image](https://jcblogimages.blob.core.windows.net/img/2013/02/image1.png "image")

I was a little quick on the CTRL + . this morning, and since the first item in the list was System.Web.WebPages.Html, the tools didn’t help me out much at all.&nbsp; But hey…shouldn’t ReSharper be able to figure this one out for me and give me the light bulb of awesome?&nbsp; 

[![image](https://jcblogimages.blob.core.windows.net/img/2013/02/image_thumb1.png "image")](https://jcblogimages.blob.core.windows.net/img/2013/02/image2.png)

I digress…

Regardless of trying to put using statements in my views for my namespace, or adding the namespace to my web.config in the views folder, the problem was I had this guy:
<pre class="csharpcode"><span class="kwrd">using</span> System.Web.WebPages.Html;</pre>
<style type="text/css">
.csharpcode, .csharpcode pre
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
.csharpcode .lnum { color: #606060; }</style>

…but I really wanted this guy:
<pre class="csharpcode"><span class="kwrd">using</span> System.Web.Mvc;</pre>
<style type="text/css">
.csharpcode, .csharpcode pre
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
.csharpcode .lnum { color: #606060; }</style>

Fixing that using in the top of my helper class resolved the issue, as now I was actually targeting the proper HtmlHelper class, in the correct namespace, that is being used by my views.&nbsp; Hope this helps someone else out there as well.