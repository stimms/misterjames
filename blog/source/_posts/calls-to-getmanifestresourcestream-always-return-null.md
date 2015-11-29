title: Calls to GetManifestResourceStream Always Return NULL
tags:
  - 'c#'
  - GetManifestResourceStream
  - reflection
id: 161
categories:
  - Develop Meta
date: 2012-12-14 17:13:00
---

I was trying to load some text from a DLL where the text was saved into text files, included in the project and marked to be compiled as an Embedded Resource. I was using the below approach from the [MSDN documentation](http://support.microsoft.com/kb/319292):
<pre class="csharpcode">    _assembly = Assembly.GetExecutingAssembly(); 
    _textStreamReader = <span class="kwrd">new</span> StreamReader(_assembly.GetManifestResourceStream(<span class="str">"MyNamespace.MyTextFile.txt"</span>));</pre>
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

I was building the namespace dynamically from the type, and I had the files in a Resources directory in my project. You can load the namespace from your type. When you put an embedded resource in a folder, you're supposed to prefix the filename with "Folder."&nbsp; I thought I was all set.

Yet, this wasn't working, so I eventually tried to eliminate any of the extra variables, inserting the namespace statically (the default namespace from my project) and moving the file into the root of my project.&nbsp; Still no love.

I built my DLL and opened it up in ILDASM.&nbsp; Guess what? No resources were being exported!&nbsp; Lamesauce!

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Calls-to_BA59/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Calls-to_BA59/image_4.png)

What what?!

So I went back into my project, tried removing and adding the files back again. I double- and triple-checked that my files were marked to be embedded resources.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Calls-to_BA59/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Calls-to_BA59/image_2.png)

Sanity check. I added a new text file to my project, called TextFile1.txt, the default in Visual Studio, set the Build Action to "Embedded Resource" and went back to my DLL.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Calls-to_BA59/image_thumb_2.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Calls-to_BA59/image_6.png)

Sweet jalapeño peppers! What was different!?

...oh.&nbsp; Right. I am using the pattern "DomainNames.**<u>en</u>**.txt", not "DomainNames.txt". Notice I had the culture in there? That can't possible make a difference, can it?&nbsp; I renamed all my txt files, rebuilt and...

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Calls-to_BA59/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Calls-to_BA59/image_8.png)

Booya!

## So Remember!

If you are trying to load resources that are compiled into your assembly as Embedded Resources, it appears you can't use extra "dotting" in your naming convention or the compilers won't properly add your file to the manifest, at which point all calls to GetManifestResourceStream will return null.