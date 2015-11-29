title: Day 2 – Adding Entities to a Storage Account Table
tags:
  - 8DaysOfTableStorage
  - Asp.Net MVC
  - Azure
  - Table Storage
id: 5521
categories:
  - Uncategorized
date: 2015-01-21 00:49:51
---

_In [this series](http://jameschambers.com/2015/01/day-0-8-days-of-working-with-azure-table-storage-from-asp-net-mvc-5/) we are looking at the basic mechanics of interacting with cloud-based Table Storage from an MVC 5 Application, using the Visual Studio 2013 IDE and Microsoft Azure infrastructure._

We’ve got some basics in place, namely loading up all resources from a table, but it turns out that users want to be able to add records, too. Who knew?&nbsp; To allow this to happen, we’re going to need to update our view a little and add an appropriate method on our controller.&nbsp; Let’s make that happen, then we’re going to take a look at why our approach is breaking down.&nbsp; Don’t worry, we’ll start fixing things up in Day 3!

If you want to follow along, please [pop into Azure](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119) and make sure you’ve got an account ready to go. The trial is free, so get at it!

## Updating Our View

We’re going to keep working off of the view we created in Day 1, just simply by adding a form below the existing UI.&nbsp; 
<pre class="csharpcode">&lt;h2&gt;Like Kittehs? Add a Kitteh!&lt;/h2&gt;
@<span class="kwrd">using</span> (Html.BeginForm(<span class="str">"Index"</span>, <span class="str">"Home"</span>, FormMethod.Post))
{
    &lt;div <span class="kwrd">class</span>=<span class="str">"form-group"</span>&gt;
        &lt;label <span class="kwrd">for</span>=<span class="str">"ImageUrl"</span>&gt;What kind of Kitteh <span class="kwrd">is</span> it?&lt;/label&gt;
        &lt;select <span class="kwrd">class</span>=<span class="str">"form-control"</span> name=<span class="str">"PartitionKey"</span>&gt;
            &lt;option <span class="kwrd">value</span>=<span class="str">"FunnyKittehs"</span>&gt;Funny Kitteh&lt;/option&gt;
            &lt;option <span class="kwrd">value</span>=<span class="str">"CuteKittehs"</span>&gt;Cute Kitteh&lt;/option&gt;
        &lt;/select&gt;
    &lt;/div&gt;
    &lt;div <span class="kwrd">class</span>=<span class="str">"form-group"</span>&gt;
        &lt;label <span class="kwrd">for</span>=<span class="str">"RowKey"</span>&gt;Kitteh Name&lt;/label&gt;
        &lt;input type=<span class="str">"text"</span> <span class="kwrd">class</span>=<span class="str">"form-control"</span> id=<span class="str">"RowKey"</span> name=<span class="str">"RowKey"</span> placeholder=<span class="str">"SeriousKitteh"</span>&gt;
    &lt;/div&gt;
    &lt;div <span class="kwrd">class</span>=<span class="str">"form-group"</span>&gt;
        &lt;label <span class="kwrd">for</span>=<span class="str">"ImageUrl"</span>&gt;Image URL&lt;/label&gt;
        &lt;input type=<span class="str">"url"</span> <span class="kwrd">class</span>=<span class="str">"form-control"</span> id=<span class="str">"ImageUrl"</span> name=<span class="str">"ImageUrl"</span> placeholder=<span class="str">"http://awesomekittehs.org/pic1.jpg"</span>&gt;
    &lt;/div&gt;
    &lt;button type=<span class="str">"submit"</span> <span class="kwrd">class</span>=<span class="str">"btn btn-default"</span>&gt;Submit&lt;/button&gt;
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

If you’re [new to Bootstrap](http://jameschambers.com/2014/08/30-days-of-bootstrap-and-asp-net-mvc-5/), it may seem like there’s a lot going on there, but you get used to it pretty quickly. Our form has three “groups” in it.&nbsp; This is a construct in Bootstrap that allows us to associate controls and labels and get the CSS styling we expect.&nbsp;&nbsp; Each form group has a label and a control, in our case, and we’re simply putting in the properties we need in order to create a new row in our Azure Table. 

The form is configured to POST to the Index action on the Home controller, so let’s set up someone to answer when the calls start coming.

## Updating Our Controller

The next piece is just as easy. We want to accept the form information (our controller will leverage MVC’s model binding) and then create a new entity in the Table. 
<pre class="csharpcode">[HttpPost]
<span class="kwrd">public</span> ActionResult Index(KittehEntity entity)
{
    var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting(<span class="str">"StorageConnectionString"</span>));

    var client = storageAccount.CreateCloudTableClient();
    var kittehTable = client.GetTableReference(<span class="str">"PicturesOfKittehs"</span>);

    var insert = TableOperation.Insert(entity);
    kittehTable.Execute(insert);

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

The insert is a type of TableOperation that we get by calling the static method aptly named “Insert” and passing in our entity. Our kittehTable object here is created in the context of a table reference that we load from our storage account by name.

At the tail end of the insert operation, we reload the list of kittehs (everyone is here to see the kittehs, after all) and return the view to render the list.

## It’s Falling Apart!

Good grief, Charlie Brown, you’ve repeated all your code!&nbsp; Our controllers should handle the mapping of requests and related payload to service endpoints, and direct users to the correct views. 

Putting application logic into our methods is a dangerous slope, and anytime our actions start doing more than one thing we start making it hard to test and maintain our software.&nbsp; This is ungood.

## Next Steps

A lot of times tutorials and blog posts fall short in the area of best practices, and I don’t want to go there for this one. In tomorrow’s exercise we’re going to break our problem down into better pieces that more accurately represent what you’d be doing in a real-world application.