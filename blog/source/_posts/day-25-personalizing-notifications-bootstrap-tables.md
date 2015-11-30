title: 'Day 25: Personalizing Notifications'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4371
categories:
  - Code Dive
date: 2014-07-01 13:45:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Back on [Day 19](http://jameschambers.com/2014/06/day-19-long-running-notifications-using-badges-and-entity-framework-code-first/) we introduced persistent storage for notifications, allowing us to pop a [custom badge](http://jameschambers.com/2014/06/day-18-customizing-and-rendering-bootstrap-badges/) up in the navbar. This was great, but as we left it, it was creating notifications only in the seed method of our DbContext migrations configuration, and the notifications were shown to all users. Not too helpful for dialing in on specific details or messages for specific users.

Today we’re going to get that “everyone’s data” out of the mix, update our notifications so that they belong to a single user and then create a temporary way for us to add new, user-specific notifications to the site.

## Clearing our DB and Seed Method

Let’s get those records (from our seed method) out of the database. Locate your DB in SQL Server Management Studio and delete the rows. Something this simple is adequate:
<pre class="csharpcode"><span class="kwrd">delete</span> <span class="kwrd">from</span> notifications</pre>
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

Now, the only thing is that effort is all in vain if we don’t clean out our seed method. Each time the database configuration class is used to perform or check for migrations it will re-insert those rows.

Locate the Configuration class, which should be in \Migrations directory in the root of your solution. Comment out or delete all the code we added to upsert the notifications.

## Extend our Notifications Class

Let’s next add a couple of properties to our Notification class, located in \Models.
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">string</span> UserId { get; set; }
<span class="kwrd">public</span> <span class="kwrd">bool</span> IsDismissed { get; set; }</pre>
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

Remember that when we modify a class that takes part in our Entity Framework works that we’ll also need to generate a migration and update the database to match our model. Type these commands into the Package Manager Console, updating the namespace appropriately:
<pre class="csharpcode">Add-Migration PersonalNotifications -ConfigurationTypeName SimpleSite.Migrations.Configuration
Update-Database -ConfigurationTypeName SimpleSite.Migrations.Identity.Configuration</pre>

The first command creates a migration for us, and the second updates the database accordingly.
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

## Updating our ActionFilter

So, if notifications belong only to users, we never want our filter to execute if the current request is for an unauthenticated user. We’ll also want to make sure that we only capture notifications for the user that _is_ logged in, and then, only the notifications that have not yet been seen.&nbsp; 

Here’s the updated code for the NotificationFilter (located in \Filters):
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">override</span> <span class="kwrd">void</span> OnActionExecuted(ActionExecutedContext filterContext)
{
    <span class="kwrd">if</span> (!filterContext.HttpContext.User.Identity.IsAuthenticated) <span class="kwrd">return</span>;

    var userId = filterContext.HttpContext.User.Identity.GetUserId();

    var context = <span class="kwrd">new</span> SiteDataContext();
    var notifications = context.Notifications
        .Where(n =&gt; n.UserId == userId)
        .Where(n =&gt; !n.IsDismissed)
        .GroupBy(n =&gt; n.NotificationType)
        .Select(g =&gt; <span class="kwrd">new</span> NotificationViewModel
        {
            Count = g.Count(),
            NotificationType = g.Key.ToString(),
            BadgeClass = NotificationType.Email == g.Key
                ? <span class="str">"success"</span>
                : <span class="str">"info"</span>
        });

    filterContext.Controller.ViewBag.Notifications = notifications;
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

## Scaffolding a Temporary Controller and View

The MVC Framework has some great scaffolding capabilities, as we’ve explored in minor detail so far. Today we’re going to use this feature to create our whole controller and the related views for all our CRUD operations. Right-click as you normally do on the controllers folder, and click Add-&gt;Controller. 

This time ‘round, use the Scaffold for the “MVC5 Controller with views, using Entity Framework”. Fill out the options as follows:

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image.png "image")

You are selecting the Notification model, checking off “Generate views” and “Use a layout page”. The controller name should automatically be set to NotificationsController for you. Click Add to finish it out.

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image1.png "image")

One more thing: you’re going to want an easy way to get your UserId – it’s what we’re using to match the notifications – so add the following to your Create view in Views\Notifications\Create.cshtml:
<pre class="csharpcode">@<span class="kwrd">if</span> (User.Identity.IsAuthenticated)
{
    &lt;p&gt;Your User ID <span class="kwrd">is</span> &lt;b&gt;@User.Identity.GetUserId()&lt;/b&gt;&lt;/p&gt;
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

That will output your UserId (which is a Guid) so that you can create notifications for yourself. 

Now when you run the site, sign in and navigate to the /Notifications path. This will show you an empty list, but you’ll have a link to create some new records. Add some to the site, using your UserId, and watch the navbar light up.

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image2.png "image")

## Next Steps

Now, I did say this is a temporary solution. By no means would you actually have a situation where you’d have users enter their own notifications, you’d more likely have events happen in the system that require some notification to be required – perhaps the completion of a job or a required update to some business process.&nbsp; That’s why we just used the scaffolding today…the NotificationsController and view are something that you’ll likely eventually just delete…and that’s okay! One of the nice things about scaffolding is that you’re not married to it. So delete it when you’re done with it.

In the real world, however, you would likely want users to be able to see and manage notifications in some way. Tomorrow we’ll look at getting the first of those bits in place.