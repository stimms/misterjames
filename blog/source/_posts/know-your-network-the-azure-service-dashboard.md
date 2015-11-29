title: Know Your Network - The Azure Service Dashboard
tags:
  - Asp.Net MVC
  - Azure
  - Azure Websites
id: 431
categories:
  - Develop Meta
date: 2012-11-23 17:22:00
---

> Having trouble viewing your Windows Azure Website? In spite of all the inner voices' assurances, it might not be you. 

# 

# A Bump in the Night

I have had a very positive experience with Windows Azure. It just works. There have been a couple of headaches along the way, but I sort of expect that since my preferred flavour of Azure has been all the juicy bits still in preview mode.

I was awakened by the furnace kicking in at 4am – it's 18 degrees Celsius below zero here right now – and was struck with some creative flair.&nbsp; I shot downstairs to my basement office to work on an Asp.Net MVC site that I'm hosting on Windows Azure Websites.

After less than 15 minutes of work (sure the flair was creative, but it was also small) I attempted a push to the cloud.&nbsp; And, yes, [Steve](https://twitter.com/srogalsky), my tests were all passing. ![Smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/wlEmoticon-smile_2.png) The response:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_2.png)

Internet Explorer cannot display the webpage? Okay, let's try Fiddler...

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_4.png)

The server did not return a response for this request? Le sigh.

# The Big Fix. Not.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_thumb_2.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_6.png)The thing I'm digging the most about Azure's tooling is how it's taking the friction away. When they introduced the "Download Publish Profile" a couple of months ago, it made the process of pushing a web site to the cloud _crazy easy_. And because your deployment settings (your publish profiles) are now part of your project, you can script this out and push to the cloud from the command prompt or even as part of your build script.

So I thought, "Hey, maybe I somehow mucked my publishing settings up?".&nbsp; I've been known to...uh....optimize the odd file or two by hand before. I cleaned out my publishing settings through the build menu (Build –&gt; Publish Selection –&gt; Profile –&gt; Manage Profiles) then headed back to my [Azure admin portal](http://manage.windowsazure.com/) to pull down my publish profile. I imported it again and hit publish.&nbsp; To the cloud!&nbsp; Gold, right? Nope. 

So, back to the portal. I knew something was going to be wrong with what I'd just done, so let's leverage the built-in tooling for Azure: logging, error details and failed request tracing. This was going to give me something for sure.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_8.png)

You set these up on your "Configure" tab from your website dashboard.&nbsp; Then you can access all the logs via FTP, the details are listed on your dashboard under the Diagnostic Logs heading in your sidebar.

Hit the page a couple times, refresh my stats. Still nada. Here's what I was seeing from the stats chart:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_thumb_4.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_10.png)

Yeah, that's right. No errors, no requests, no failed connections to the DB.&nbsp; Zilch.&nbsp; This is exactly what I want to see at any other time in production _except when I'm **actually** getting errors_.

Enough with this cloud business...time to kick it old school. I've been around the block and have run into similar things on IIS when there was a mal-formed config file.&nbsp; Visual Studio didn't have anything for me as far as errors or warnings, so I darted over to [W3Schools](http://www.w3schools.com/dom/dom_validate.asp) to validate the document there. This was a long shot as my site was running locally, but I thought it was worth it.&nbsp; Still, everything checked out and this failure is not due to my tag skills or lack thereof.

I headed back to my web.config and dropped in this classic, hoping it would spice up my error life:
<pre class="csharpcode">  <span class="kwrd">&lt;</span><span class="html">customErrors</span> <span class="attr">mode</span><span class="kwrd">="Off"</span> <span class="kwrd">/&gt;</span></pre>
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

Still no love. I thought I was going crazy.&nbsp; Technically, that didn't have any relevance in regards to this problem, but I like to say that every now and then to keep my ego in check.

I muddled around a bit more in vain, determined to figure out what it was I busted.&nbsp; An hour and a half in, certain that I had crossed all my i's and dotted all my t's, I finally decided to open a support ticket.&nbsp; The menu option for this is on your portal when you click on your account name:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_thumb_5.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_12.png)

And then the trophy appeared:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_thumb_6.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_14.png)

You can verify the health of the Windows Azure Platform on the [Service Dashboard](http://www.windowsazure.com/en-us/support/service-dashboard/).&nbsp; Oh how I wish I had known that two hours earlier.&nbsp; Check this out:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_thumb_7.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Know-Your-Network_5415/image_16.png)

Guess which overtired, otherwise confident blurry-eyed developer hosts his Website in West US?

# The Moral of the Story

I actually didn't know that there was a service dashboard.&nbsp; This is great. I've now pinned the site to my home screen on my Windows Phone, and the next time I wake up at 4 AM I'm going to check that little baby first before heading down for 15 minutes of Renaissance-style mid-night creativity.&nbsp; So, I don't have a moral to share here, just another tool for the war chest.

Before you pull your hair out fixing "your" problem next time, make sure your hosting provider is giving you all the sweet goods first!

Finally, if there are any shiny happy Windows Azure team members reading this, could you please, _please_ add a notification on my dashboard if a service I am using is experiencing stress, a midlife crisis or apparent loss of network connectivity?&nbsp; Thanks so much!

(p.s. By the time I finished writing this post, the [Service Dashboard](http://www.windowsazure.com/en-us/support/service-dashboard/) showed green checks for all my service friends)