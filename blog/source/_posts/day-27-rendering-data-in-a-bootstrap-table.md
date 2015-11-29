title: 'Day 27: Rendering Data in a Bootstrap Table'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4501
categories:
  - Code Dive
date: 2014-07-03 22:23:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

We created a nice tab for our users to be able to manage the different components of their account, hi-jacking the Manage Account feature of the default MVC template. Now we want to render the user’s outstanding notifications in a good looking table in one of those tabs.

**Caution**: this is the **_lipstick on a pig_** post of the series. Here be dragons…

## Rendering Tables with Bootstrap

The first thing you need to know is that you don’t have to throw away a single thing you’ve learned in your years of HTML ninja skill building. Tables are still not to be used for layout. Tables are for lists of data. And more importantly, Bootstrap doesn’t try to change the semantics or document structure for a table. In fact, to get the Bootstrap look-and-feel in an HTML table, you need to apply but one class.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">table</span> <span class="attr">class</span><span class="kwrd">="table"</span><span class="kwrd">&gt;</span>
  ...
<span class="kwrd">&lt;/</span><span class="html">table</span><span class="kwrd">&gt;</span></pre>
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

So what else does it offer?&nbsp; A bunch of common things that you might try to do, like “[tighter](http://getbootstrap.com/css/#tables-condensed)” versions of a table, [striped styling](http://getbootstrap.com/css/#tables-striped) or [hover-states](http://getbootstrap.com/css/#tables-hover-rows). You can also use the now familiar [contextual classes](http://getbootstrap.com/css/#tables-contextual-classes) on rows or cells to highlight pertinent information to your users.

All in all, it’s a clean looking table that will suit most needs and fit in with the rest of the site.

[![image](http://jameschambers.com/wp-content/uploads/2014/07/image_thumb1.png "image")](http://jameschambers.com/wp-content/uploads/2014/07/image4.png)

Another important aspect of the Bootstrap table is the ability to easily make a table responsive. I don’t have the answer as to why this isn’t the default, but it’s easily added with a DIV wrapper that has the table-responsive class. This adds horizontal scroll bars to your tables, when needed, when they are viewed on smaller screens to ensure that the data doesn’t end up in some non-accessible, off-device part of the screen.

[![image](http://jameschambers.com/wp-content/uploads/2014/07/image_thumb2.png "image")](http://jameschambers.com/wp-content/uploads/2014/07/image5.png)

That wrapper looks like the following:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="table-responsive"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">table</span> <span class="attr">class</span><span class="kwrd">="table table-striped "</span><span class="kwrd">&gt;</span>
       <span class="rem">&lt;!-- table rows here --&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">table</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
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

## Retrieving the Notifications

We’re going to need to get the list of notifications that haven’t yet been dismissed. You may have already created some test data with the efforts from the past few days, and that will work great here. 

Open the AccountController.cs class and navigate down to the Manage action.&nbsp; Just before it, add the following code :
<pre class="csharpcode"><span class="kwrd">private</span> IEnumerable&lt;Notification&gt; GetUserNotifications()
{
    <span class="rem">// get the user ID</span>
    var userId = User.Identity.GetUserId();

    <span class="rem">// load our notifications</span>
    var context = <span class="kwrd">new</span> SiteDataContext();
    var notifications = context.Notifications
        .Where(n =&gt; n.UserId == userId)
        .Where(n =&gt; !n.IsDismissed)
        .Select(n =&gt; n)
        .ToList();
    <span class="kwrd">return</span> notifications;
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

All this guy is doing, really, is snapping up the rows for this currently logged in user. 

One thing to mention here is that we’re really now _crying_ for some Dependency Injection. Our NotificationFilter and our AccountController are now both creating instances of our SiteDataContext – now multiple instances per request – and it’s making our code harder to test.

## Adding the Data to Our ViewBag

Both the GET and POST versions of Manage are already relying on ViewBag to get data up to the view, so we’ll follow the same cue and put our notifications in there. In both methods you’ll find the ReturnUrl being assigned to a ViewBag property, so immediately after that, add the following line of code:
<pre class="csharpcode">ViewBag.NotificationList = GetUserNotifications();</pre>
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

## Adding a Partial View for the Table

Next, let’s get a partial view added called “_RenderNotifications.cshtml”.&nbsp; The model type for the page will be a IEnumerable&lt;Notification&gt;, and we’ll iterate over the collection of rows to generate the TR and relevant TDs inside each of those. The entire view will look something like this:
<pre class="csharpcode">@using SimpleSite.Models
@model IEnumerable<span class="kwrd">&lt;</span><span class="html">Notification</span><span class="kwrd">&gt;</span>

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="table-responsive"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">table</span> <span class="attr">class</span><span class="kwrd">="table table-striped "</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">tr</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">th</span><span class="kwrd">&gt;</span>Type<span class="kwrd">&lt;/</span><span class="html">th</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">th</span><span class="kwrd">&gt;</span>Notification<span class="kwrd">&lt;/</span><span class="html">th</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">th</span><span class="kwrd">&gt;</span>Actions<span class="kwrd">&lt;/</span><span class="html">th</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">tr</span><span class="kwrd">&gt;</span>
        @foreach (var notification in Model)
        {
            var badgeClass = NotificationType.Email == notification.NotificationType
                ? "label-success"
                : "label-info";
            <span class="kwrd">&lt;</span><span class="html">tr</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">td</span><span class="kwrd">&gt;&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="label @badgeClass"</span><span class="kwrd">&gt;</span>@notification.NotificationType<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;&lt;/</span><span class="html">td</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">td</span><span class="kwrd">&gt;</span>@notification.Title<span class="kwrd">&lt;/</span><span class="html">td</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">td</span><span class="kwrd">&gt;&lt;/</span><span class="html">td</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">tr</span><span class="kwrd">&gt;</span>
        }
    <span class="kwrd">&lt;/</span><span class="html">table</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>

There’s a placeholder in there for “Actions”, which we’ll look at tomorrow when we revisit buttons and explore drop-downs.
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

## Updating our Manage View

With the partial view in place and the data being loaded into the ViewBag we’re ready to update our view. Return to the Manage.cshtml file and locate our placeholder for the notifications. Update it to render the partial view, passing in the collection of notifications.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-pane active"</span> <span class="attr">id</span><span class="kwrd">="notifications"</span><span class="kwrd">&gt;</span>
    @Html.Partial("_RenderNotifications", ViewBag.NotificationList as IEnumerable<span class="kwrd">&lt;</span><span class="html">Notification</span><span class="kwrd">&gt;</span>)
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>

That should be it! I found what seems to be a bug in Bootstrap where if you have a table in a tab, and the tab is set to active and you have the fade class, the table doesn’t seem to be visible on page load. It shows up fine after clicking on a tab, but to work around this you’ll notice that I’ve removed the fade class from the tab.

## Oh Noes! It’s Turned Into A Mess!

Folks, I’m not going to lie; though the Account controller and Manage views may have never have been intended to handle our humble little notifications, I found several things in both the controller and related views that left me uncomfortable with the design. I’m trying to keep this as a “how to get some MVC in your Bootstrap” type series, but all of the following were worth noting as things I’d try to avoid:

*   Nested views with non-obvious dependencies
*   Multiple methods that do the same work as each other
*   Magic strings
*   Methods doing too many things or things that you wouldn’t attribute of them by their name (for example, the “Manage” action which resolves status messages based on optional parameters)
*   ViewBag use that gets in the way of the view’s maintainability
*   Views that don’t leverage Bootstrap enough
*   Way too much reliance on ViewBag, and, therefore,
*   Absence of reasonable view models
*   Dogs and cats playing together

I will perhaps one day sit down and hash through the AccountController, but needless to say, there’s _some_ amount of work to be done! I, for one, would like to see more best practices in place, a more referencable example of how to do things and sample views that better embrace the Bootstrap visuals (since, after all, it’s included by default in the template).

Now on our end, we’ve done some off-script things as well, namely putting direct database access in a controller (and filter),&nbsp; pushing database models directly up to the view and leaning on those ViewBag properties to shuffle data around. So, yes, kettle, meet teapot.

All of these things are leading up to another set of posts on best practices ![Smile](http://jameschambers.com/wp-content/uploads/2014/07/wlEmoticon-smile.png).

## Next Steps

Okay, our users can now see the list of notifications that are outstanding, but they have no way to manage them just yet. Tomorrow we’ll get some drop-down button action in play and explore some of the ways we can compose some pushable UI.