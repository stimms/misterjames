title: jQuery Script Map Causing Critical Error in jQuery 1.9.0
tags:
  - Asp.Net MVC
  - jQuery
id: 1591
categories:
  - Code Dive
date: 2013-01-29 16:08:52
---

Script maps are a great addition to the debugging arsenal of any script developer. Even for those of us who are “consumers” of some of the libraries out there, the script maps will help us get at the details of why something is going sideways when we’re working with minified versions of the code.&nbsp; 

Without the map, it has been difficult to find the correlating code in the original file. Elijah Manor does a great job at going over how to take advantage of script map support and it’s [benefits in jQuery 1.9.0](http://www.elijahmanor.com/2013/01/the-magic-of-jquery-source-map.html).

## Minification in MVC4 and Critical Errors from the jQuery Script Map

There is a chance that you’ll run into this if you’re dropping code in Visual Studio 2012 on Win 8 with Asp.Net MVC, using bundling and minification:
 > JavaScript critical error at line 1, column 11 in [http://localhost:0000Scripts/jquery-1.9.0.min.map](http://localhost:0000Scripts/jquery-1.9.0.min.map)
> 
> SCRIPT1004: Expected ';' 

If this is the case, check to see that your wildcard isn’t to greedy. For example, if you have something like this set up in your BundleConfig.cs:
<pre class="csharpcode">    bundles.Add(<span class="kwrd">new</span> ScriptBundle(<span class="str">"~/bundles/jquery"</span>).Include(
                <span class="str">"~/Scripts/jquery-1.*"</span>));</pre>

…you’re telling the bundling engine to grab anything matching the script “jquery-1.anything”, and as a great optimization, the MVC Framework is smart enough to grab only one of library.js or library.min.js, and it prefers the min.js for you.&nbsp; The downside here is that the bundle builder is using a prefix match here, and it seems to be picking up the .map file as well. This seems to be inadvertently causing the critical error mentioned above (map files aren’t to be shipped to the client as part of the request).

## The Fix is In

The good thing is that you’re not stuck on wildcards, and it’s likely better to be using the semantic versioning replacement token (especially with the jQuery libraries).&nbsp; Update your bundle to the following:
<pre class="csharpcode">    bundles.Add(<span class="kwrd">new</span> ScriptBundle(<span class="str">"~/bundles/jquery"</span>).Include(
                <span class="str">"~/Scripts/jquery-{version}.js"</span>));</pre>
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

The {version} token is a replacement that does a best match against the file according to [SemVer](http://semver.org/).&nbsp; Now, instead of all the files getting added, and the risk of files being added multiple times, we get a smarter match on the library we are requesting for bundling and minification.