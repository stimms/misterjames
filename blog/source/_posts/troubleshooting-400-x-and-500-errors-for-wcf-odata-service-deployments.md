title: Troubleshooting 400.x and 500 Errors for WCF OData Service Deployments
tags:
  - OData
  - WCF
  - Web Development
  - Web Services
id: 1981
categories:
  - Develop Meta
date: 2013-11-04 04:14:23
---

> Running into 400 &amp; 500 error messages for WCF OData Services can be tricky to get past and even harder to sort through the red herrings out there. Here’s a few tips to attack any error messages you might run into. 

Sweet! All done your spanky new OData services using Entity Framework in the back end and you’re feeling pretty awesome because a) it was stupid-easy to get going, and, #2 you are now the happy owner of a JSON data source with querystring filtering capabilities. Only trouble is, that darned thing won’t tick on your deployment server.

Don’t sweat it. There’s a dozen things that can sideways, but they’re all easy to fix.

## Check Your Pre-Requisites

If you’re going to host over HTTP, you’ll obviously have IIS installed, but make sure you light up all the right pieces. You’ll need .Net 3.0 and 4.5 installed and enabled. If you want HTTP activation, that may not be on by default.&nbsp; This [MSDN article will get you there](http://msdn.microsoft.com/en-us/library/ms731053.aspx).

## Try the Catch-Alls

Some of the errors you’ll run into can most easily be solved by using what’s given to you for free.&nbsp; Check the Windows Event logs and drill into the details

If you’re not seeing much fruit from those efforts, you can also configure your service to use the verbose mode of error detail. This will likely reveal the juiciest information and is as easy as adding one line of code to your InitializeService method, as follows:
<pre class="csharpcode">config.UseVerboseErrors = <span class="kwrd">true</span>;</pre>
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

## Double-Check Your Configuration

If you’ve moved from dev to test or prod, you’ll need to make sure you update your connection strings.&nbsp; Nestled in several of those 400 error message are details about a problem connecting to the database or other insight related to your credentials or connections through EF in your EDMX.

## Some Low Hanging Fruit

I’ve run into 500’s a couple times, no information, just “internal server error”. This was because the application pool wasn’t set up for the correct version of .Net. Use the Advanced settings for your site in IIS to choose the app pool, or navigate IIS to the Application Pools and create a new one that matches the version of .Net for your service.

If you get 404.3, this is likely related to missing either the correct .Net components or the WCF Activation components (see [MSDN article](http://msdn.microsoft.com/en-us/library/ms731053.aspx)).

401’s that I’ve seen have been permissions related, naturally as they’re 401’s. ![Smile](https://jcblogimages.blob.core.windows.net/img/2013/11/wlEmoticon-smile.png) Check your directory permissions if you’re doing something on the file system, and your DB credentials in your web.config. Remember that those EDMX’s have some [funky formatting](http://msdn.microsoft.com/en-us//library/cc716756.aspx).

## Learn More

If you’ve got a night to invest in yourself I highly recommend reading the deeper bits on WCF Data Services. If you get serious, you’ll want to look into:

*   [Securing](http://msdn.microsoft.com/en-us/library/dd728284(v=vs.110).aspx) services, and
*   [Interceptors](http://msdn.microsoft.com/en-us/library/dd744842(v=vs.110).aspx) to implement business logic.

An indispensible tool in your kit is [LINQPad](http://www.linqpad.net/) – a must have for working with WCF OData.