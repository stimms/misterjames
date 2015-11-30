title: Pushing Data to an Azure Service Bus Queue
tags:
  - Azure
  - Queues
  - ServiceBus
id: 6051
categories:
  - Code Dive
date: 2015-04-15 02:29:55
---

This is a simple example of what you need to get data into a queue in Azure Service Bus from a console application. This uses the WindowsAzure.ServiceBus NuGet package, so you can actually use it anywhere you can store a connection string and write some .Net.

[Go here to get started](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119), or log into your Azure portal.

## Setting up the Queue

First, we’re going to create a new Service Bus Queue through the portal. This is pretty easy, just select New –&gt; App Services –&gt; Queue and pick Custom Create.

[![image](https://jcblogimages.blob.core.windows.net/img/2015/04/image_thumb1.png "image")](https://jcblogimages.blob.core.windows.net/img/2015/04/image1.png)

Punch in a name, create a new namespace then click next. You can use the default settings, just hit confirm. Note that if you use Quick Create, it assumes you have a namespace already setup, if that’s the case, you can use that as well.

Navigate to the Namespace dashboard and go to the configure tab. You’ll need to note the **shared access key name** and the **key value** itself. 

## Building the App

Next, create a new Console App in Visual Studio. Add this configuration line to your App.config (just make sure to remove the whitespace, it wrecks your config):
<pre class="csharpcode">    <span class="kwrd">&lt;</span><span class="html">add</span> <span class="attr">name</span><span class="kwrd">="AzureWebJobsServiceBus"</span>
         <span class="attr">connectionString</span>=
            <span class="kwrd">"Endpoint=sb://NAMESPACE.servicebus.windows.net/;
             SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=KEY"</span> <span class="kwrd">/&gt;</span></pre>
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

This gives the framework the connection string you need via the conventionally named key/value pair. Punch in those details you grabbed from the portal so that the connection string is valid for your environment.

Next, add a simple person class to your project as such:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> Person
{
    <span class="kwrd">public</span> <span class="kwrd">string</span> Name { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">string</span> EmailAddress { get; set; }
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

## Get The Bits You Need

Now, get ready to send the message. You want to add the following package to your program via the Package Manager Console (or the Manage Packages menu item by right-clicking on your project in Solution Explorer).
<pre class="csharpcode">install-package WindowsAzure.ServiceBus</pre>
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

## Put in the Code that Does the Heavy Lifting

Finally, update your program code to grab the connection string and publish the message to the queue.
<pre class="csharpcode"><span class="kwrd">static</span> <span class="kwrd">void</span> Main(<span class="kwrd">string</span>[] args)
{
    var connectionString = ConfigurationManager.ConnectionStrings[<span class="str">"AzureWebJobsServiceBus"</span>].ConnectionString;
    var client = QueueClient.CreateFromConnectionString(connectionString, <span class="str">"NAME-OF-QUEUE"</span>);
    var person = <span class="kwrd">new</span> Person {Name = <span class="str">"James Chambers"</span>, EmailAddress = <span class="str">"james@foo.com"</span>};
    client.Send(<span class="kwrd">new</span> BrokeredMessage(person));
}
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

That’s it. Four lines of code and you have a way to push out a message and get back to work. Heck, if you can await (as in, in an aysnc method) you can even use SendAsync instead of Send so you don’t tie up your thread.

## Next Steps

In the next post I’ll show you how to pop stuff off the queue and do something with it, then we’ll have a look at how to debug all of this.

Make sure you [pop into Azure](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119) and give this a try! There is an amount of free credits that you get during your trial period. If you want to extend that, consider [getting into MSDN](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575136) (or getting your boss to) for other cool things as well, like VS Online and access to over a terabyte of development assets &amp; software.

Happy Coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/04/wlEmoticon-smile2.png)
