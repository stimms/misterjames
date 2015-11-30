title: 'Day 19: Long-Running Notifications Using Badges and Entity Framework Code First'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4021
categories:
  - Code Dive
date: 2014-06-24 00:58:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

We previously looked at using TempData to store alerts, which is great when what we are trying to alert the user to is a transient message that lives only for the next request. What about the scenario where we have long running notifications? Unread email? Outstanding actions the user must follow up on?

Today we’re going to bang out a solution here…in the interest of time and simplicity this _will not_ be in line with best practices, but come back tomorrow for a discussion on how to get closer to them.

## Rocking Out A Model

If we want a way to indicate that the user needs to take care of something, we need two pieces: some kind of UI to point it out – which we have with badges – and some place to store the data – which we’ll get with EF.&nbsp; Right-click on your Models directory and add a new class, called Notification, and add the following code to it:
<pre class="csharpcode">    <span class="kwrd">public</span> <span class="kwrd">enum</span> NotificationType
    {
        Registration,
        Email
    }

    <span class="kwrd">public</span> <span class="kwrd">class</span> Notification
    {
        <span class="kwrd">public</span> <span class="kwrd">int</span> NotificationId { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> Title { get; set; }
        <span class="kwrd">public</span> NotificationType NotificationType { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> Controller { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> Action { get; set; }
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

These are _just_ classes, but when we lean on Entity Framework, we get a database out of it as well. In the “code first” approach, we first write ourselves a class that represents the data, as we’ve done above. Next, we have to let EF know that we expect a storage mechanism for that data, namely, we create a data context and a DB set (representing a table) inside of another class. Add SiteDataContext as a class in your Models folder and add the following code to it:
<pre class="csharpcode">    <span class="kwrd">public</span> <span class="kwrd">class</span> SiteDataContext : DbContext
    {
        <span class="kwrd">public</span> SiteDataContext() : <span class="kwrd">base</span>(<span class="str">"DefaultConnection"</span>) { }

        <span class="kwrd">public</span> DbSet&lt;Notification&gt; Notifications { get; set; }
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

There’s not much to it: DbSet represents a table and DbContext identifies this as a class that represents a connection to the database. If the DB doesn’t exist, EF creates it for us. We call the base class constructor with a string, which identifies the connection string to use in the web.config file, if present, or the name of the DB that will be created if it doesn’t.

At this point, **build your project **using Shift+Ctrl+B or by pressing F6\. We need the app compiled to take advantage of tooling in Entity Framework.

Now, go to the Package Manager Console (View –&gt; Other Windows –&gt; Package Manager Console) and type the following command:
<pre class="csharpcode">Enable-Migrations -ContextTypeName SimpleSite.Models.SiteDataContext</pre>

You’ll need to make sure that the namespace and class name match what is specified above. This command generates a configuration file that has an empty Seed method (rather, it has some comments in it, but you can delete them).&nbsp; This file can be used by Entity Framework as a way to do advanced configuration, set your own conventions for table naming, or, in our case, seeding the database with some test data.&nbsp; Paste in the following code in the Seed method:
<pre class="csharpcode"><span class="kwrd">protected</span> <span class="kwrd">override</span> <span class="kwrd">void</span> Seed(SimpleSite.Models.SiteDataContext context)
{
    context.Notifications.AddOrUpdate(notification =&gt; notification.Title,
        <span class="kwrd">new</span> Notification
        {
            Title = <span class="str">"John Smith was added to the system."</span>,
            NotificationType = NotificationType.Registration
        },
        <span class="kwrd">new</span> Notification
        {
            Title = <span class="str">"Susan Peters was added to the system."</span>,
            NotificationType = NotificationType.Registration
        },
        <span class="kwrd">new</span> Notification
        {
            Title = <span class="str">"Just an FYI on Thursday's meeting"</span>,
            NotificationType = NotificationType.Email
        });
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

AddOrUpdate is like an “upsert” method you can use to inject or update data in your DB.&nbsp; It accepts a lambda expression that specifies how unique rows are identified, and a parameter array which is a list of objects you want to inject into the specified table. 

## Updating our Database

With support for the migrations in place, we want to create an initial version of the DB and apply it in our development environment. From the Package Manager Console, type these lines of code:
<pre class="csharpcode">Add-Migration db-create

Update-Database</pre>
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

Have a look through the migration class that is generated. Pretty cool, eh? Although this is just a first stab at it, it really shows how you can customize the build up – or tear down – of you tables. It’s worth a whole book of content, though, so I’m not diving in for now! The call to Update-Database executes the Up() method on all outstanding migrations. Everything is tracked for you by EF in the database. 
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

## Okay, Here’s One Best Practice

One thing that I really like about MVC and EF is how easy it is to get data into your view using the entities of your database. One thing that I really hate about MVC and EF is how easy it is to get data into your view using the entities of your database. For reals. It’s great for demos and terrible for production.

Instead, use a view model…a way to decouple your view from your database completely. Get in this habit as early as you can, and then come back and thank me 6 months into the maintenance portion of your contract. 

Add another class called NotificationViewModel and write in the following code:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> NotificationViewModel
{
    <span class="kwrd">public</span> <span class="kwrd">int</span> Count { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">string</span> NotificationType { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">string</span> BadgeClass { get; set; }
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

Rather than relying on the data in the database we’re going to use a projection of the data instead. This can help to shield our UI from changes in the model and eliminate the risk of excess DB access or over-exposing sensitive data. It also allows for other best practices we’ll cover in the days ahead.

## Updating Our Controller

Update your Index() action to be the following:
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult Index()
{
    var context = <span class="kwrd">new</span> SiteDataContext();

    var notifications = context.Notifications
        .GroupBy(n =&gt; n.NotificationType)
        .Select(g =&gt; <span class="kwrd">new</span> NotificationViewModel
        {
            Count = g.Count(),
            NotificationType = g.Key.ToString(),
            BadgeClass = NotificationType.Email == g.Key
                ? <span class="str">"success"</span>
                : <span class="str">"info"</span>
        });

    ViewBag.Notifications = notifications;

    <span class="kwrd">return</span> View();
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

We’re getting the list of notifications out of the DB, grouping them by type, specifying the style class to use and counting them op.&nbsp; We then set that information in the ViewBag, which is accessible from our views. Note that this approach _only_ works for now in our Index method, and _only_ on the HomeController. Don’t worry, we’ll take care of that tomorrow. ![Smile](https://jcblogimages.blob.core.windows.net/img/2014/06/wlEmoticon-smile2.png)

## Lighting up Notifications in the View

Find the UL element with the class “nav navbar-nav” that contains the LI elements that compose the menu (these are around line 26 for me). At the end of the LI elements, we’re going to inject a few more, so we iterate over the list of notifications that we popped into the ViewBag like so:
<pre class="csharpcode">@<span class="kwrd">foreach</span> (NotificationViewModel notification <span class="kwrd">in</span> ViewBag.Notifications)
{
    &lt;li&gt;&lt;a href=<span class="str">"#"</span>&gt;@notification.NotificationType &lt;span <span class="kwrd">class</span>=<span class="str">"badge badge-@notification.BadgeClass"</span>&gt;@notification.Count&lt;/span&gt;&lt;/a&gt;&lt;/li&gt;
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

We’ve got some more work to do to fully get these working, but you will likely see the direction this is moving in by now, and users will see that something needs to be done about all those…notifications.

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image37.png "image")

## Next Steps

Well, as I said we’ve got some clean up to do to get a little closer to best practices. Our models are in our web site project and unable to be reused. We have to put code in each and every single action on every controller where we would want to see notifications. Our layout is getting polluted with additional code. There’s no way to resolve notifications. We’re in bad shape!

Check back tomorrow so that we can resolve some of the low hanging fruit and discuss what remains to get some of this train on the rails.