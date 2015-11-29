title: Sharing With Friends - Opening up IIS Express To Other Team Members
tags:
  - Android
  - ASP.NET
  - IIS
  - Mobile
  - Visual Studio 2010
id: 781
categories:
  - Code Dive
date: 2012-04-12 17:56:00
---

> I'm developing an app with an Android-ian friend. When co-developing some of the features, we've found it critical to be able to easily access the development stream of the services, which I'm building with Asp.Net's WebAPI. Here's an easy way to configure your IIS Express instance to allow outside traffic. 

## Playing Inside the Sandbox

The out-of-box experience for IIS Express is to have all web traffic closed to clients not residing on localhost. This is a great security practice, but not great when you're_ trying_ to share a site or service.&nbsp; Configuration of IIS Express is also non-evident; the little UI you do have exposes no way for you to actually modify the section of config that allows access.

To enable access outside your box, you need to create a virtual directory for your app and edit the applicationhost.config file located My Documents\IIS Express\config.

In Visual Studio, double-click on your project properties and open up the Web tab. Scroll down to the section where you can select the server and make sure it's on IIS Express. Finally, click on the Create Virtual Directory button. This creates an entry in the config file.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Sharing-With-Friends---Opening-up-IIS-Ex_1241D/image_5.png "image")

Now, open up that config file (again, it's in My Documents\IIS Express\config) and find your newly created site.&nbsp; Update the config section so that it looks something the the following by adding a second binding. 
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">site</span> <span class="attr">name</span><span class="kwrd">="MySiteOfAwesomeness"</span> <span class="attr">id</span><span class="kwrd">="13"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">application</span> <span class="attr">path</span><span class="kwrd">="/"</span> <span class="attr">applicationPool</span><span class="kwrd">="Clr4IntegratedAppPool"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">virtualDirectory</span> <span class="attr">path</span><span class="kwrd">="/"</span> <span class="attr">physicalPath</span><span class="kwrd">="C:\dev\MySiteOfAwesomeness\MySiteOfAwesomeness"</span> <span class="kwrd">/&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">application</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">bindings</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">binding</span> <span class="attr">protocol</span><span class="kwrd">="http"</span> <span class="attr">bindingInformation</span><span class="kwrd">="*:4111:localhost"</span> <span class="kwrd">/&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">binding</span> <span class="attr">protocol</span><span class="kwrd">="http"</span> <span class="attr">bindingInformation</span><span class="kwrd">="*:4111:192.168.100.101"</span> <span class="kwrd">/&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">bindings</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">site</span><span class="kwrd">&gt;</span>
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

Note that instead of localhost you can use your computer name or your IP address.&nbsp; 

Finally, you're going to need to open up that port through whatever firewall you have.&nbsp; For me, I used Windows Firewall with Advanced Security (much better than the Marginal Security version) and added a rule to allow traffic on my project's port.

Hope this helps someone, and, hey, be nice to your Android friends, they might be somebody's brother.