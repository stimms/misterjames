title: 'Day 21: Cleaning Up Filtering, the Layout & the Menu'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4081
categories:
  - Code Dive
date: 2014-06-26 01:18:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

In software development we often talk about concepts like the Single Responsibility Principle. Web pages are often doing _lots_ of things, so it’s hard to say that it should always apply when we’re building our views. But, in the spirit of Martin Fowler’s take on it, I would argue that our _Layout.cshtml is starting to get a lot of reasons as to why it might change, and for that reason, we’re going to split out the menu.

## Extracting the Menu

In your Views\Shared folder add another partial called _MenuPartial.cshtml and paste in the following code.
<pre class="csharpcode">@using SimpleSite.Models
<span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="nav navbar-nav"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>@Html.ActionLink("Home", "Index", "Home")<span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>@Html.ActionLink("About", "About", "Home")<span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>@Html.ActionLink("Contact", "Contact", "Home")<span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>@Html.ActionLink("My Peeps", "Index", "Simple")<span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>

    @if (ViewBag.Notifications != null)
    {
        foreach (NotificationViewModel notification in ViewBag.Notifications)
        {
            <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>
                    @notification.NotificationType
                    <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="badge badge-@notification.BadgeClass"</span><span class="kwrd">&gt;</span>
                        @notification.Count
                    <span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        }
    }
<span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span></pre>
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

If you flip back to our _Layout view, you’ll see that this is very close to what we had there. I’ve made two important changes:

1.  I’ve added a link to our SimpleController in the menu.
2.  I’m checking to make sure there is data in the ViewBag before accessing the dynamic property. This will prevent errors should there be no notifications for the users.

If you do #1 without #2 above, you will most definitely get errors because the only place that has notifications injected is in the Index action on the Home controller.

With those bits in place, be sure to pop back into your _Layout, and update the navbar where we had previously added the code for notifications. It should now only include a call to render our _MenuPartial and the _LoginPartial.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="navbar-collapse collapse"</span><span class="kwrd">&gt;</span>
    @Html.Partial("_MenuPartial")
    @Html.Partial("_LoginPartial")
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>

If you run the site at this point you won’t get any errors, but you’ll only see the notifications on the home page. Let’s address that.

## Globally Registering an Action Filter

We’re now going to set it up so that our NotificationFilter is executed on every request. I just ask that you remember that this is a demo, and that there are [a few caveats you should be aware of](http://jameschambers.com/2014/06/day-20-an-actionfilter-to-inject-notifications/).

During application startup (checkout your global.asax in the root of the site) you’ll notice a call to FilterConfig.RegisterGlobalFilters. There is no magic here. There’s a static method on a class located in your App_Start folder that helps to keep that global.asax nice and tidy. A GlobalFilterCollection is passed in and we can then add to it. 

In previous versions of the MVC template, this wasn’t around, so most folks ended up either dropping in a ton of lifting into Application_Start or otherwise coming up with a comparable solution to the above. Now, the class that does the FilterConfig does the FilterConfig. Kinda like that whole Single Responsibility Principle again, eh?

Update FilterConfig (which has the global error handling baked in already) to also include the registration of our filter:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">static</span> <span class="kwrd">void</span> RegisterGlobalFilters(GlobalFilterCollection filters)
{
    filters.Add(<span class="kwrd">new</span> HandleErrorAttribute());
    filters.Add(<span class="kwrd">new</span> NotificationFilter());
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
Now, return to the Home Controller and remove the NotificationFilter attribute from the Index action.&nbsp; It won’t really matter if you don’t (the Framework is smart enough to see that it’s already in play) but you might confuse the future version of yourself when you disable the global registration down the road and the filter keeps getting executed.

You’re all set!

## Next Steps

One final step that you might want to consider in the clean up is to move your notification model to a separate DLL, but I’ll leave that as an exercise to the reader as it’s more of a “code” thing and less of an “MVC or Bootstrap” thing.

Next up, we’ll tackle user registration and membership.