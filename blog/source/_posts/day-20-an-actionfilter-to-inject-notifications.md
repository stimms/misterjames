title: 'Day 20: An ActionFilter to Inject Notifications'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4061
categories:
  - Code Dive
date: 2014-06-25 02:08:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Yesterday we added some good-looking notification badges to our site using our [customized Bootstrap badges](http://jameschambers.com/2014/06/day-18-customizing-and-rendering-bootstrap-badges/). Now we need to take care of this nonsense where we need to put the same code into _every single method and every single controller _that we write.

[![image[5]](https://jcblogimages.blob.core.windows.net/img/2014/06/image5_thumb.png "image[5]")](https://jcblogimages.blob.core.windows.net/img/2014/06/image51.png)

Yay for the badges, boo for the code. To get around this, we’ll explore one of many different ways we solve this problem of repeating ourselves by writing an action filter.

## A Bit About the MVC Pipeline

There’s a lot going on behind the scenes in a web request, but thankfully much of it is abstracted away for most development tasks, and nearly all of it is abstracted away for the end user (could you imagine trying to explain to Mom or Dad how to manually resolve the IP address of domain name? Yikes!).

Sometimes you need to dive into the abstraction, and action filters are one area where this is the case. Remember back on [day one](http://jameschambers.com/2014/06/day-1-the-mvc-5-starter-project/) of this series I broke down the request a little? And on [day two](http://jameschambers.com/2014/06/day-2-examining-the-solution-structure/) I introduced some of the terminology? Well…if you’ve been following along, you can probably infer what an action filter might be up to: before or after executing an action on a controller you get to inspect, prod, poke and otherwise modify the content or even redirect the user as you see fit. From [MSDN](http://msdn.microsoft.com/en-us/library/gg416513(vs.98).aspx):
 > Action filters. These implement [IActionFilter](http://msdn.microsoft.com/en-us/library/system.web.mvc.iactionfilter(v=vs.98).aspx) and wrap the action method execution. The [IActionFilter](http://msdn.microsoft.com/en-us/library/system.web.mvc.iactionfilter(v=vs.98).aspx) interface declares two methods: [OnActionExecuting](http://msdn.microsoft.com/en-us/library/system.web.mvc.iactionfilter.onactionexecuting(v=vs.98).aspx) and [OnActionExecuted](http://msdn.microsoft.com/en-us/library/system.web.mvc.iactionfilter.onactionexecuted(v=vs.98).aspx).[OnActionExecuting](http://msdn.microsoft.com/en-us/library/system.web.mvc.iactionfilter.onactionexecuting(v=vs.98).aspx) runs before the action method. [OnActionExecuted](http://msdn.microsoft.com/en-us/library/system.web.mvc.iactionfilter.onactionexecuted(v=vs.98).aspx) runs after the action method and can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method. 

You can deny access, restrict availability of resources and execute arbitrary code. You even have access to internal parts of MVC, so your filter can be aware of the currently executing controller, action, and target view.

And that’s where we’re going to hook in: just before the view is rendered after the action method has run.

## Adding the Filter

Add a folder to the root of your site called Filters (this isn’t convention, just a way to help keep you organized) and then add a class called NotificationFilter. Inherit from ActionFilterAttribute, and then override the OnActionExecuted method so that you can add your two cents to the request.&nbsp; Your code will be nearly identical to what you put in the Home controller, except note now that the ViewBag property is actually accessed through the Controller property on your filterContext.
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">override</span> <span class="kwrd">void</span> OnActionExecuted(ActionExecutedContext filterContext)
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

ActionFilterAttribute gives you four virtual methods for actions and results on executing and executed. You only have to implement the ones that suit your fancy.&nbsp; Because it’s an attribute class, you can decorate your actions (or even your classes) with it and the MVC Framework will pick it up and execute it when the time is right.

## Touch up the Controller

Now on your HomeController we can clean things up a fair bit. Remove almost all the code from the Index action and decorate it with your new attribute.
<pre class="csharpcode">[NotificationFilter]
<span class="kwrd">public</span> ActionResult Index()
{
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

Much cleaner, eh? You can now run your site and you’ll get the same result as we had before, and the notifications will now appear anywhere you place that attribute. If you put it at the class level, all actions on the controller will have the filter applied.&nbsp; Which means I should probably mention…

## …Some Notes and Caveats

This is a pretty powerful deal, especially at the class level. Heck, you can even globally register your filter and have it execute on every request. Which is why I have to stress some pretty important bits:

*   If it’s _always_ executing, then it’s _always_ executing. Be mindful of decorating your classes as a rule or registering filters globally, especially if you have requests (AJAX) that maybe don’t need the filter.&nbsp; Alternatively, design your filter in such a way that it knows when it should or shouldn’t run.
*   Lots-of-stuff-going-on doesn’t scale well. Use filters judiciously so that you’re not bogging your site down with unnecessary operations. Be quick in what you do so that you return quickly and keep your performance up. Use profiling if you’re not sure how fast your code is running.
*   In this example – and I can get away with it because it’s an example – I’m going to the database on each request. It may suit you well to do this, but consider alternate approaches (such as caches with timeouts and operations that invalidate them) so that you’re not making those hits all the time.

## Next Steps

We still have a few things to clean up from [Day 19](http://jameschambers.com/2014/06/day-19-long-running-notifications-using-badges-and-entity-framework-code-first/), so we’ll keep working away at some housekeeping tasks tomorrow, working on our layout. 