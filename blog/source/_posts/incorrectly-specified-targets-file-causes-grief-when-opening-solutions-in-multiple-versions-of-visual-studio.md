title: Incorrectly Specified Targets File Causes Grief When Opening Solutions in Multiple Versions of Visual Studio
tags:
  - Asp.Net MVC
  - Visual Studio 2010
  - Visual Studio 2012
id: 1931
categories:
  - Develop Meta
date: 2013-09-09 00:52:00
---

Working with solutions under different versions of Visual Studio is a welcome feature that has, for the most part, gone very smoothly for me.&nbsp; There are some common sense environmental limitations and the ability to open projects in different versions of VS doesn't save you from missing tools (I use RazorGenerator, for example), but otherwise it's been a treat.

However, last night I got snagged for about 20 minutes when I ran into the following error when trying to open a project:
 > The imported project "C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v10.0\WebApplications\Microsoft.WebApplication.targets" was not found. Confirm that the path in the declaration is correct, and that the file exists on disk. 

Without getting too deep into the details (which you [can read here](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx)), Visual Studio injects some properties to your project file in order to maintain compatibility of your project in different versions of VS. In my case, environmentally, my system wasn't configured in a way that was compatible with the properties that were created in the process of opening the project originally.&nbsp; The solution was to simply remove the following from the project file:
<pre class="csharpcode">  <span class="kwrd">&lt;</span><span class="html">PropertyGroup</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">VisualStudioVersion</span> <span class="attr">Condition</span><span class="kwrd">="'$(VisualStudioVersion)' == ''"</span><span class="kwrd">&gt;</span>10.0<span class="kwrd">&lt;/</span><span class="html">VisualStudioVersion</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">VSToolsPath</span> <span class="attr">Condition</span><span class="kwrd">="'$(VSToolsPath)' == ''"</span><span class="kwrd">&gt;</span>$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)<span class="kwrd">&lt;/</span><span class="html">VSToolsPath</span><span class="kwrd">&gt;</span>
  <span class="kwrd">&lt;/</span><span class="html">PropertyGroup</span><span class="kwrd">&gt;</span></pre>
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

### Is This a Miss?

I was a little frustrated by the process, especially because the error, while entirely accurate, couldn't give me more details on how to fix it. Without some digging online and coming across Sayed's blog, I wouldn't have been able to resolve this.

In situations like this, I don't want to have to fight my tools, and I'm including the code my tools generate as "tools" as well. A very simple fix would be to also insert a comment into the project file along with the property group describing the intent and perhaps a KB article link to give details on the subject. 

### Why Did You Run Into This?

I think it's easy to see how this could happen in my scenario: I have multiple laptops with multiple OS's running multiple versions of IDE's. The OS and the IDE are sometimes betas or otherwise advance copies. My projects are created and opened in any combination of these. That said, I was unable to recreate this problem on Win7/8 using VS2010/2012.

### Wrapping Up...

It seems that removing this element from the csproj was enough to satisfy the IDE. There are other solutions, such as copying in valid targets files from other working environments. As I'm using Visual Studio 2012 exclusively on this project going forward, removing the VisualStudioVersion and VSToolsPath properties from my project file is a solution that worked for me.