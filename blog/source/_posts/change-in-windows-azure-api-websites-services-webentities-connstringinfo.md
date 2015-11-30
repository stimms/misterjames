title: Change in Windows Azure API - Websites.Services.WebEntities.ConnStringInfo
tags:
  - Azure Web Apps
id: 5861
categories:
  - Code Dive
date: 2015-04-04 21:58:23
---

Automation is the bomb. I don’t like having to do something that can be automated more than once, so usually if I’m asked a second time (and I have a strong suspicion that I’ll be asked to do it again) I try to script it. This is very true of Windows Azure Web Apps and thankfully, it’s also easily facilitated.

I just fired up some old Azure PowerShell scripts I had (from mid-2013, I believe) that had a call that looked a little like the following:
<pre class="csharpcode"><span class="rem"># create and configure the connection string information </span>
$appConnString = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.ConnStringInfo; 
$appConnString.Name=<span class="str">"PersonContext"</span>; 
$appConnString.ConnectionString=$appDBConnStr; 
$appConnString.Type=<span class="str">"SQLAzure"</span>; </pre>
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

Essentially, it’s just creating an instance of the ConnStringInfo object so I can associate it with my site. However, when I tried running it I started getting the following error:

> New-Object : Cannot find type [Microsoft.WindowsAzure.Management.Utilities.Websites.Services.WebEntities.ConnStringInfo]: 
> verify that the assembly containing this type is loaded.

Then, because the object couldn’t be created, I got errors trying to set each of the properties, like, “The property 'ConnectionString' cannot be found on this object. Verify that the property exists and can be set.”

Then I noticed that the script really mustn’t have been run in some time, as this is the current namespace for the class:

> Microsoft.WindowsAzure.**<u>Commands</u>**.Utilities.Websites.Services.WebEntities.ConnStringInfo

Websites was moved in late 2013 to the Commands namespace. You won’t likely run into it unless you dig up an old script that hasn’t run in a while, but the difference is subtle and might be tricky to spot.

Hope this helps, and happy coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/04/wlEmoticon-smile.png)
