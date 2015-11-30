title: 'Day 30: Loading Bootstrap Modal Content via AJAX'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4711
categories:
  - Code Dive
date: 2014-07-20 05:58:00
---

This is **the final** installment in a 30 day series on Bootstrap and the MVC Framework. To see the rest of the series be sure to check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/). I hope you have enjoyed following along! ![Smile](https://jcblogimages.blob.core.windows.net/img/2014/07/wlEmoticon-smile1.png)

Here in our last post in the series we’re going to revisit our modal dialog confirmation box that pops up when a user tries to delete a record. At this point, regardless of the notification selected for delete, the user will see the same static dialog.

## Loading Remote Content With Bootstrap’s Modal

There is built-in support for Bootstrap to load content into a modal, but there are some limitations. First, Bootstrap loads the content the first time the constructor is called on the modal. Secondly, the modal’s constructor is only ever called once, meaning, you can’t refresh the content without some extra script.

Rather than trying to put a square peg in a round hole, we can take the simpler approach of just loading the content with jQuery. After all, it’s clean, easy to understand, and at the end of the day the modal is just HTML.

This approach will work better for us for the following use case:

*   User selects to delete a notification
*   They cancel out of the dialog
*   The select a different notification to delete 

At this point, the remote loading capabilities of Bootstrap’s modal break down and we’d have to get into hacky stuff that may not survive a minor version upgrade. Pass! In my opinion, the modal should have an overload that accepts a ‘remote url’ parameter.

If, however, you have a use case that doesn’t involve refreshing the dialog, I encourage you to check out [the docs](http://getbootstrap.com/javascript/#modals-usage) for using the built-in functionality.

## Improving our Delete Confirmation

Rather than just giving a generic message, we’re going to instead load some content via AJAX. This will give us a chance to put a visual preview of the data that will be deleted in front of the user.

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image14.png "image")

To accomplish this we’re going to need to make small change to our modal markup (in Views\Account\_RenderNotifications.Modal.cshtml), as follows:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-body"</span> <span class="attr">id</span><span class="kwrd">="notificationPreview"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>Loading content...<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
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

What I’ve done here is removed the content from the modal-body section of the markup and replaced it with a loading message.&nbsp; I’ve also given the DIV an ID so that we can address it from JavaScript more directly.&nbsp; Also, revisit your Views\Account\_RenderNotifications.js.cshtml and update the confirmDelete method, as we’re now going to clear the contents and load the text dynamically.
<pre class="csharpcode"><span class="kwrd">function</span> confirmDelete(id) {
    currentNotificationId = id;
    previewContainer.html(<span class="str">'&lt;p&gt;Loading content...&lt;/p&gt;'</span>);
    previewContainer.load(confirmUrl, { id: currentNotificationId });
    $(<span class="str">'#deleteConfirmModal'</span>).modal(<span class="str">'show'</span>);
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

To pull that all together we need a couple more variable declarations at the top of the script. Here’s my final list of vars:
<pre class="csharpcode"><span class="kwrd">var</span> readUrl = <span class="str">'@Url.Action("MarkNotificationAsRead")'</span>;
<span class="kwrd">var</span> deleteUrl = <span class="str">'@Url.Action("Delete")'</span>;
<span class="kwrd">var</span> confirmUrl = <span class="str">'@Url.Action("GetNotification")'</span>;
<span class="kwrd">var</span> previewContainer = $(<span class="str">'#deleteConfirmModal #notificationPreview'</span>);
<span class="kwrd">var</span> currentNotificationId;</pre>
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

You’ll notice that I have added a confirm URL and captured the element (in previewContainer) that we are using for the confirm. This saves me from having to requery the DOM on each display of the dialog.

## Adding the Controller Action

We’re going to use that same pattern for loading up the notification for the confirmation that we used for modifying and deleting. Let’s add a GetNotification method to our AccountController that has the following code:
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult GetNotification(<span class="kwrd">int</span> id)
{
    var userNotification = GetUserNotifications().FirstOrDefault(n =&gt; n.NotificationId == id);

    <span class="kwrd">if</span> (userNotification == <span class="kwrd">null</span>)
    {
        <span class="kwrd">return</span> <span class="kwrd">new</span> HttpNotFoundResult();
    }

    <span class="kwrd">return</span> PartialView(<span class="str">"_RenderNotifications.ModalPreview"</span>, userNotification);
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

This time, we’ll be returning a partial view, as indicated in the code above, that will help us render the relevant content for the user to make their choice.

But hey…that code to load a single notification for the logged in user is _crying_ for some refactoring, am I right?

## Adding the Partial View

The controller is wired to pass the single notification to a partial view, which will essentially have the same information we had in the content section of the modal markup, but now we’ll also render in some notification-specific information. You can use your imagination on how this could be further extended with more complex objects in your own project.

Here are the contents of _RenderNotifications.ModalPreview.cshtml, which you should put into Views\Account:
<pre class="csharpcode">@model SimpleSite.Models.Notification

<span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>You have selected to delete this notification.<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="panel panel-warning"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="panel-heading"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">h4</span><span class="kwrd">&gt;</span>@Model.Title<span class="kwrd">&lt;/</span><span class="html">h4</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="panel-body"</span><span class="kwrd">&gt;</span>
            If this was the action that you wanted to do,
            please confirm your choice, or cancel and return
            to the page.
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span></pre>
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

## Next Steps

Wow! Thirty days of Bootstrap and MVC! It’s been a long 30 days…50 days actually, but no one could have foreseen the passing of my Grandma, my three-day fever of 103 degrees or the fact that this would end up running into my vacation for my 14th wedding anniversary and _a most excellent trip_ to Seattle with my high school sweetheart (to whom I am married).

I have a few more things to add to this series, but for now we’ll call it a wrap. Thanks for your questions, suggestions, feedback and comments along the way!

Happy coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2014/07/wlEmoticon-smile1.png)
