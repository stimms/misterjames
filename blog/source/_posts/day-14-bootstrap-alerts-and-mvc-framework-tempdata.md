title: 'Day 14: Bootstrap Alerts and MVC Framework TempData'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3721
categories:
  - Code Dive
date: 2014-06-15 03:30:49
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

* * *
   <p>_This post is dedicated to the memory of my Grandmother, Marianne Chambers, who passed away June 14, 2014 with her two of her daughters at her side. She was a great woman who survived the passing of her husband and the early loss of a son, my father._

* * *
   <p>Something may go wrong, or it may go right. It may go smooth or go bump in the night. And, at various times, your application will want to let the user know about it. **Bootstrap** has a component called an Alert that works quite nicely for these types of information.

[![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image_thumb16.png "image")](https://jcblogimages.blob.core.windows.net/img/2014/06/image27.png)

Likewise, the **MVC Framework** has a great place where we can store these messages without having to worry about postbacks, short term storage or the like. Pairing these two features up makes a great way to relay information to the user.

## A Few More Helpers

Add another class file to your project in the Helpers namespace, and drop in the following code:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> Alert
{
    <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> TempDataKey = <span class="str">"TempDataAlerts"</span>;

    <span class="kwrd">public</span> <span class="kwrd">string</span> AlertStyle { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">string</span> Message { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">bool</span> Dismissable { get; set; }
}

<span class="kwrd">public</span> <span class="kwrd">static</span> <span class="kwrd">class</span> AlertStyles
{
    <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Success = <span class="str">"success"</span>;
    <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Information = <span class="str">"info"</span>;
    <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Warning = <span class="str">"warning"</span>;
    <span class="kwrd">public</span> <span class="kwrd">const</span> <span class="kwrd">string</span> Danger = <span class="str">"danger"</span>;
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

These classes give us some constants to help render our Bootstrap alerts, as well as a class to help store and pass data around.

## Base Controller – _Almost_ A Best Practice

One of the first things you will likely do on any MVC project is to start with a new base controller that you will inherit from instead of the built-in Controller that ships with the framework. 

While that may seem like a weird suggestion coming from a guy who likes the YAGNI principle, I’ve never worked on a project that didn’t have a base controller, end up with one, or at least couldn’t have used one.&nbsp; There will be valid cases where you won’t need one, and likely several cases where multiple base controllers will be required, so it’s debatable as to whether or not it should be a rule.

This part is a best practice, however: don’t let your base classes get “fat”, loaded with code you’re not using 90% of the time.

At any rate, if you choose to use them or not, for this project ours will look like this:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> BaseController : Controller
{
    <span class="kwrd">public</span> <span class="kwrd">void</span> Success(<span class="kwrd">string</span> message, <span class="kwrd">bool</span> dismissable = <span class="kwrd">false</span>)
    {
        AddAlert(AlertStyles.Success, message, dismissable);
    }

    <span class="kwrd">public</span> <span class="kwrd">void</span> Information(<span class="kwrd">string</span> message, <span class="kwrd">bool</span> dismissable = <span class="kwrd">false</span>)
    {
        AddAlert(AlertStyles.Information, message, dismissable);
    }

    <span class="kwrd">public</span> <span class="kwrd">void</span> Warning(<span class="kwrd">string</span> message, <span class="kwrd">bool</span> dismissable = <span class="kwrd">false</span>)
    {
        AddAlert(AlertStyles.Warning, message, dismissable);
    }

    <span class="kwrd">public</span> <span class="kwrd">void</span> Danger(<span class="kwrd">string</span> message, <span class="kwrd">bool</span> dismissable = <span class="kwrd">false</span>)
    {
        AddAlert(AlertStyles.Danger, message, dismissable);
    }

    <span class="kwrd">private</span> <span class="kwrd">void</span> AddAlert(<span class="kwrd">string</span> alertStyle, <span class="kwrd">string</span> message, <span class="kwrd">bool</span> dismissable)
    {
        var alerts = TempData.ContainsKey(Alert.TempDataKey)
            ? (List&lt;Alert&gt;)TempData[Alert.TempDataKey]
            : <span class="kwrd">new</span> List&lt;Alert&gt;();

        alerts.Add(<span class="kwrd">new</span> Alert
        {
            AlertStyle = alertStyle,
            Message = message,
            Dismissable = dismissable
        });

        TempData[Alert.TempDataKey] = alerts;
    }

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

These methods are going to help us record and render alerts from our controller and into our views.&nbsp; There are four pretty similar calls going on there that keep the helper strings away from our controllers.&nbsp; The AddAlert method takes care of fetching or creating a list of alerts.

We’re using TempData here for storage which is good for the next _completed_ request from the server to the same client. That could be the current request, should we complete execution and render a view, or that could be the immediately _next_ request, should we decide to redirect the user.

Using a list of Alerts gives us the ability to add more than one type of alert, or several instances of alerts to the page at once. If you consider things like ActionFilters (those are coming) that have an opportunity to interact with the execution pipeline, you won’t ever know exactly what parts of your application might be trying to signal something to the user.

**Note**: I got the idea for this approach through working with [Eric Hexter](https://twitter.com/ehexter) on the [Twitter.Bootstrap.Mvc](https://github.com/erichexter/twitter.bootstrap.mvc) project. In that version, Eric’s implementation allowed for one message per alert type, but it was definitely what got the ball rolling for me on this one.

## Updates to PersonController

Start by changing the declaration to inherit from our spanky new base class.
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> PersonController : BaseController</pre>
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

Then, update your create method to use the new methods we’ve added.
<pre class="csharpcode">[HttpPost]
<span class="kwrd">public</span> ActionResult Create(Person person)
{
    <span class="kwrd">if</span> (ModelState.IsValid)
    {
        _people.Add(person);
        Success(<span class="kwrd">string</span>.Format(<span class="str">"&lt;b&gt;{0}&lt;/b&gt; was successfully added to the database."</span>, person.FirstName), <span class="kwrd">true</span>);
        <span class="kwrd">return</span> RedirectToAction(<span class="str">"Index"</span>);
    }
    Danger(<span class="str">"Looks like something went wrong. Please check your form."</span>);
    <span class="kwrd">return</span> View(person);
}
</pre>

We’ve made it super easy to store various alerts which will hang around in memory until we complete a request. In the case of the happy path above, the Success message hangs around on the server until the client requests the redirected content.&nbsp; For the Danger message on the sad path, the view is immediately returned and the TempData is cleared.

## Showing our Alerts

While we could just put all the info we need into the layout, we can keep our code cleaner by using a partial view to render the alerts. Under Views\Shared, add a new partial view called _Alerts.cshtml and put in the following code:
<pre class="csharpcode">@{
    var alerts = TempData.ContainsKey(Alert.TempDataKey)
                ? (List&lt;Alert&gt;)TempData[Alert.TempDataKey]
                : <span class="kwrd">new</span> List&lt;Alert&gt;();

    <span class="kwrd">if</span> (alerts.Any())
    {
        &lt;hr/&gt;
    }

    <span class="kwrd">foreach</span> (var alert <span class="kwrd">in</span> alerts)
    {
        var dismissableClass = alert.Dismissable ? <span class="str">"alert-dismissable"</span> : <span class="kwrd">null</span>;
        &lt;div <span class="kwrd">class</span>=<span class="str">"alert alert-@alert.AlertStyle @dismissableClass"</span>&gt;
            @<span class="kwrd">if</span> (alert.Dismissable)
            {
                &lt;button type=<span class="str">"button"</span> <span class="kwrd">class</span>=<span class="str">"close"</span> data-dismiss=<span class="str">"alert"</span> aria-hidden=<span class="str">"true"</span>&gt;&amp;times;&lt;/button&gt;
            }
            @Html.Raw(alert.Message)
        &lt;/div&gt;
    }
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

We grab the alerts out of the TempData (if they exist) and then loop through each one, rendering them in the order they were added. If they had the dismissable flag set to true, we also render the appropriate Bootstrap elements and styles to make the alert go away on command.

Now update your _Layout.cshtml to include the call to render the partial view. Your container with the call to RenderBody() should now look like this:
<pre class="csharpcode">&lt;div <span class="kwrd">class</span>=<span class="str">"container body-content"</span>&gt;
    @{ Html.RenderPartial(<span class="str">"_Alerts"</span>); }
    @RenderBody()
    &lt;hr /&gt;
    &lt;footer&gt;
        &lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt;
    &lt;/footer&gt;
&lt;/div&gt;</pre>
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

And now, after you add a new person to the list, you’ll see a nicely formatted, dismissable alert at the top of the page.

[![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image_thumb17.png "image")](https://jcblogimages.blob.core.windows.net/img/2014/06/image28.png)

## Next Steps

We’ve been jumping around a bit and starting to form a few ideas about how the Asp.Net MVC Framework and Bootstrap can work together, but perhaps it’s time to look at a few of the basics that are already in use with the default template.