title: Day 1 – The Basics of the Basics with Azure Table Storage
tags:
  - 8DaysOfTableStorage
  - Azure
  - MVC5
id: 5271
categories:
  - Code Dive
date: 2015-01-14 13:50:00
---

_In [this series](http://jameschambers.com/2015/01/day-0-8-days-of-working-with-azure-table-storage-from-asp-net-mvc-5/) we are looking at the basic mechanics of interacting with cloud-based Table Storage from an MVC 5 Application, using the Visual Studio 2013 IDE and Microsoft Azure infrastructure._ 

#### Before We Get Going…

Let’s not mince words here, the best way to develop in the cloud is with the best and most up-to-date toolset that we have available. You’re going to want to grab these prerequisites as a minimum foundation for following along in this series:  

*   An Azure subscription (or a [free trial](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119)) <li>A copy of Visual Studio 2013 – remember that the community edition is free if you don’t own a copy! ([http://www.visualstudio.com/en-us/news/vs2013-community-vs.aspx](http://www.visualstudio.com/en-us/news/vs2013-community-vs.aspx))  <li>The latest Azure SDK for Visual Studio ([http://azure.microsoft.com/en-us/develop/net/](http://azure.microsoft.com/en-us/develop/net/)) <p>Visual Studio 2013 gives us great templates to start from and by augmenting it with the SDK we also get a rich set of cloud-management features, which we’ll explore in the later parts of the series.  

#### The Straight-Up Attack: Accessing Table Storage from Your Controller
 <p>Follow these quick steps to get started:  

*   Create a new solution in Visual Studio using the ASP.NET Web Application project template  <li>Right-click on your project in Solution Explorer, then Manage NuGet Packages and add “WindowsAzure.Storage” to your dependencies.  <li>Alternatively, you can install the package from the Package Manager Console with the following command: <pre>Install-Package WindowsAzure.Storage</pre>
<p>The project is now prepped, so now we can add the needed code bits to start building our app. Let’s start by adding the following class to our project in the Models folder: 
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> KittehEntity : TableEntity
{
    <span class="kwrd">public</span> KittehEntity(<span class="kwrd">string</span> kittehType, <span class="kwrd">string</span> kittehName)
    {
        <span class="kwrd">this</span>.PartitionKey = kittehType;
        <span class="kwrd">this</span>.RowKey = kittehName;
    }

    <span class="kwrd">public</span> KittehEntity() {}

    <span class="kwrd">public</span> <span class="kwrd">string</span> ImageUrl { get; set; }
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

Next, replace the code in the HomeController with the code below. Note that you’ll need to add the namespace for the models to the usings at the top. 
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult Index()
{
    var storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting(<span class="str">"StorageConnectionString"</span>));

    var client = storageAccount.CreateCloudTableClient();

    var kittehTable = client.GetTableReference(<span class="str">"PicturesOfKittehs"</span>);
    <span class="kwrd">if</span> (!kittehTable.Exists())
    {
        kittehTable.Create();

        var thrillerKitteh = <span class="kwrd">new</span> KittehEntity(<span class="str">"FunnyKittehs"</span>, <span class="str">"ThrillerKitteh"</span>);
        thrillerKitteh.ImageUrl = <span class="str">"http://cdn.buzznet.com/assets/users16/crizz/default/funny-pictures-thriller-kitten-impresses--large-msg-121404159787.jpg"</span>;

        var pumpkinKitteh = <span class="kwrd">new</span> KittehEntity(<span class="str">"FunnyKittehs"</span>, <span class="str">"PumpkinKitteh"</span>);
        pumpkinKitteh.ImageUrl = <span class="str">"http://rubmint.com/wp-content/plugins/wp-o-matic/cache/6cb1b_funny-pictures-colur-blind-kitteh-finded-yew-a-pumikin.jpg"</span>;

        var batchOperation = <span class="kwrd">new</span> TableBatchOperation();

        batchOperation.Insert(thrillerKitteh);
        batchOperation.Insert(pumpkinKitteh);
        kittehTable.ExecuteBatch(batchOperation);

    }

    var kittehQuery = <span class="kwrd">new</span> TableQuery&lt;KittehEntity&gt;()
        .Where(TableQuery.GenerateFilterCondition(<span class="str">"PartitionKey"</span>, QueryComparisons.Equal, <span class="str">"FunnyKittehs"</span>));

    var kittehs = kittehTable.ExecuteQuery(kittehQuery).ToList();

    <span class="kwrd">return</span> View(kittehs);
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

Admittedly, this is a bit of a naive approach because we’re mixing concerns here and putting storage access and table seeding in a single controller method, but I want to highlight the premise here without adding too much cruft. Through the rest of the series we’ll extract a service to do the heavy lifting and move back towards best practices. 
<p>What we’re doing above is the following: 

*   Reading the connection string from our web.config<li>Configuring the client<li>Getting a reference to a table<li>If the table doesn’t exist (it won’t, first time), we build a batch operation and seed it with some data<li>We fetch all records with a “FunnyKittehs” partition key<li>We pass the data to the view
<p>Now, replace the code in the Views\Home\Index.cshtml with the following: 
<pre class="csharpcode">@model IEnumerable&lt;YourNamespace.Models.KittehEntity&gt;
@{
    ViewBag.Title = <span class="str">"Home Page"</span>;
}

@<span class="kwrd">foreach</span> (var kitteh <span class="kwrd">in</span> Model)
{
    &lt;div <span class="kwrd">class</span>=<span class="str">"jumbotron"</span>&gt;
        &lt;h4&gt;@kitteh.RowKey&lt;/h4&gt;
        &lt;img src=<span class="str">"@kitteh.ImageUrl"</span> /&gt;
    &lt;/div&gt;
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

Note that you’ll need to update the namespace to match your project’s. 
<p>Finally, add a connection string to the AppSettings section of your web.config. This will tell our app to use the local storage emulator provided by the SDK. 
<pre class="csharpcode">&lt;appSettings&gt;
  &lt;!-- other settings... --&gt;
  &lt;add key=<span class="str">"StorageConnectionString"</span> <span class="kwrd">value</span>=<span class="str">"UseDevelopmentStorage=true;"</span> /&gt;
&lt;/appSettings&gt;</pre>

Finally, run the app! You should see a couple of adorable kittehs in your browser, rendered in a razor view, with data populated by the MVC controller with entities loaded from Azure Storage Tables. 
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

#### Next Steps

<p>In this post we injected a few entities into our table using a seed method, but what about adding new entities from the UI? In the next article, we’ll beef up the views and controller to allow users to add new documents to our storage account. 
<p>_(Psst! If you’re just getting started with MVC and want to get your hands <u>real</u> dirty, check out my book on _[_Bootstrap and the MVC Framework_](http://jameschambers.com/2014/08/30-days-of-bootstrap-and-asp-net-mvc-5/)_)._