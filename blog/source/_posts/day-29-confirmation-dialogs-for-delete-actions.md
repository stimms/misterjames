title: 'Day 29: Confirmation Dialogs for Delete Actions'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4661
categories:
  - Code Dive
date: 2014-07-19 22:57:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

We just gave our users the ability to delete a record from the database in [Day 28](http://jameschambers.com/2014/07/day-28-doing-more-interesting-things-with-buttons/), but a single click does the deed without confirmation. It would likely be better to at least give them a little prompt to confirm that this is what they were trying to do in the first place.

![image](http://jameschambers.com/wp-content/uploads/2014/07/image11.png "image")

Let’s first talk about how dialogs are composed and how to display them in a page.

## Bootstrap Modal Dialogs

Modals are made up of a wrapper, an outer and inner container and three common sections that provide default styling and handle proper rendering: the header, the body and the footer. You can represent the basic structure of the modal as follows:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-dialog"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-content"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-header"</span><span class="kwrd">&gt;</span>...<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-body"</span><span class="kwrd">&gt;</span>...<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-footer"</span><span class="kwrd">&gt;</span>...<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
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

You can show a dialog by using a button with an attribute that Bootstrap is aware of:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">class</span><span class="kwrd">="btn"</span> <span class="attr">data-toggle</span><span class="kwrd">="modal"</span> <span class="attr">data-target</span><span class="kwrd">="#theModal"</span><span class="kwrd">&gt;</span>
  Show my modal
<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span></pre>
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

Or you can invoke the modal by calling it from JavaScript:
<pre class="csharpcode">$(<span class="str">'#deleteConfirmModal'</span>).modal(<span class="str">'show'</span>);</pre>
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

For other events, defaults and other JavaScript method calls that are available, be sure to check out the docs on the [Bootstrap site](http://getbootstrap.com/javascript/#modals).

When using modals you’ll want as little interference as possible with the styles and layout of the elements being used, so the recommendation is to put the container for the modal as close to the root of the document structure as possible. 

One thing that I’ve run into when using the button approach is the need to have the CSS selector on the data-target attribute (see the code above). I’m not sure why this is the approach, but if you forget that you need the hashtag as a prefix to the ID, your modal won’t display. Regardless, we’re going to be using JavaScript to do our work today, but keep that tip in mind as you start working with this on&nbsp; your own.

## The Game Plan

For this exercise we’re going to hit a couple of areas of the project, so here’s a quick checklist if you want to follow along. We’re going to:

*   Clean up our JavaScript and put it in a separate file
*   Add an optionally rendered section to our _Layout page
*   Add a partial view for the markup needed for our dialog
*   Update the Manage view to include our new bits

## Some Cleanup, Some JavaScript

We’re going to start growing our client-side scripts on the view that renders notifications a little and one thing I like to do is to keep those bits organized. The JavaScript we have so far to give some UX for notifications is in our _RenderNotifications partial view, which presents some limitations as to when script can execute and whether or not jQuery has been loaded. 

One easy way to keep these things straight, and organized in our project, is to add a __**PartialViewName**_.js.cshtml file to our project in the same folder as our view. We can then consider the JavaScript related to the view as another asset that needs to be loaded on the full view.

So let’s do that. Add a new view to the Views\Account folder called _RenderNotificationsjs. If you use the scaffolding tools, you’ll notice that you can’t properly name it (it complains about invalid characters), so add it as I’ve shown, then rename it to _RenderNotifications.js.cshtml once the file is there.&nbsp; Next, remove the old JavaScript from the _RenderNotifications.cshtml and put it in the new .js.cshtml file.
<pre class="csharpcode">&lt;script type=<span class="str">"text/javascript"</span>&gt;

    <span class="kwrd">var</span> readUrl = <span class="str">'@Url.Action("MarkNotificationAsRead")'</span>;
    <span class="kwrd">var</span> deleteUrl = <span class="str">'@Url.Action("Delete")'</span>;
    <span class="kwrd">var</span> currentNotificationId;

    <span class="kwrd">function</span> updateNotification(id, action) {
        $(<span class="str">"#notificationFormItemId"</span>).val(id);
        <span class="kwrd">switch</span> (action) {
        <span class="kwrd">case</span> <span class="str">'read'</span>:
            $(<span class="str">"#notificationForm"</span>).attr(<span class="str">'action'</span>, readUrl).submit();
            <span class="kwrd">break</span>;
        <span class="kwrd">case</span> <span class="str">'delete'</span>:
            $(<span class="str">"#notificationForm"</span>).attr(<span class="str">'action'</span>, deleteUrl).submit();
            <span class="kwrd">break</span>;
        <span class="kwrd">default</span>:
            console.debug(<span class="str">'Unknown action '</span> + action);
        }
    }

    <span class="kwrd">function</span> confirmDelete(id) {
        currentNotificationId = id;
        $(<span class="str">'#deleteConfirmModal'</span>).modal(<span class="str">'show'</span>);
    }

    $(<span class="kwrd">function</span>() {
        $(<span class="str">"#deleteConfirmModal"</span>).on(<span class="str">'click'</span>, <span class="str">"#deleteConfirm"</span>, <span class="kwrd">function</span>() {
            updateNotification(currentNotificationId, <span class="str">'delete'</span>);
        });
    });

&lt;/script&gt;</pre>
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

Remember that .cshtml gives us a little more power than just a straight .js file. We can do things like interact with the ViewBag or the page Model, write code that is parsed by Razor and make use of Html helper methods, all of which can be quite handy.

There’s not too much more going on here than there was before, except that now we capture the ID of the notification in our confirmDelete method, and we are wiring up a click handler for the modal dialog that we’re going to implement.

One final thing to do on our partial is to update the Delete button (the A tag) to call our new JavaScript method as follows:
<pre class="csharpcode">&lt;a href=<span class="str">"javascript:confirmDelete(@notification.NotificationId)"</span>&gt;Delete&lt;/a&gt;</pre>
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

## Rendering at the Root Using Optional Sections in our Layout

Remember the note about keeping the modal as close to the document root as possible? Well, there’s a problem there when we’re down at the view or partial view layer, stuck inside our other “main content” area of the page. We’re inside all these other containers already and we can’t break out without a little help from the MVC Framework.&nbsp; What we want to do is to add another optional section, much like the scripts section. We can call this topLevel for now, though you’d likely want it to be more meaningful in a real project.&nbsp; Add the following code to your _Layout file.
<pre class="csharpcode">@RenderSection(<span class="str">"topLevel"</span>, required: <span class="kwrd">false</span>)</pre>
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

It should appear right after the BODY tag, first thing in the document, like this.

![image](http://jameschambers.com/wp-content/uploads/2014/07/image12.png "image")

Now, whenever you have something that will be injected from a view into the page in this topLevel section, you will be able to put elements directly into the root of the document.

## Create the Modal Partial

Next up, we’re going to create a _RenderNotification.Modal.cshtml partial view that has the HTML for our modal in it. This will help to keep our core view simple, and keep our related files together in our project.&nbsp; The markup for the modal follows the same basic structure I highlighted above and adds a few other elements to the mix.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal fade"</span> <span class="attr">id</span><span class="kwrd">="deleteConfirmModal"</span> <span class="attr">tabindex</span><span class="kwrd">="-1"</span> <span class="attr">role</span><span class="kwrd">="dialog"</span> <span class="attr">aria-labelledby</span><span class="kwrd">="deleteLabel"</span> <span class="attr">aria-hidden</span><span class="kwrd">="true"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-dialog"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-content"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-header"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">h4</span> <span class="attr">class</span><span class="kwrd">="modal-title"</span> <span class="attr">id</span><span class="kwrd">="deleteLabel"</span><span class="kwrd">&gt;</span>Deleting a Notification<span class="kwrd">&lt;/</span><span class="html">h4</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-body"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>You have selected to delete this notification.<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
                    If this was the action that you wanted to do,
                    please confirm your choice, or cancel and return
                    to the page.
                <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="modal-footer"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-success"</span> <span class="attr">data-dismiss</span><span class="kwrd">="modal"</span><span class="kwrd">&gt;</span>Cancel<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-danger"</span> <span class="attr">id</span><span class="kwrd">="deleteConfirm"</span><span class="kwrd">&gt;</span>Delete Notification<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
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

There’s a class ‘fade’ in there that tells bootstrap to slide and fade the modal in from the top. There are a few ARIA attributes in there for accessibility.&nbsp; I’ve added a title to the header, content in the body and buttons to either dismiss the dialog (cancelling the delete) or to confirm the operation when the user elects to delete the notification.

Perhaps the most interesting bit in there is the data-dismiss attribute on the cancel button, which tells Bootstrap that this dialog can be hidden when the button is clicked.

## Updating Our View

Finally, we update our view, adding the modal to the page and including the JavaScript that we need to pull it all together.

At the bottom of Manage.cshtml in Views\Account we add the topLevel section to the view and in it we render the modal markup from the partial view we created. 
<pre class="csharpcode">@section topLevel{

    @{ Html.RenderPartial(<span class="str">"_RenderNotifications.Modal"</span>); }

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

Immediately after that, we update our scripts section to include the JS we need and created above.
<pre class="csharpcode">@section Scripts { 

    @Scripts.Render(<span class="str">"~/bundles/jqueryval"</span>) 
    @{ Html.RenderPartial(<span class="str">"_RenderNotifications.js"</span>); }

}</pre>

Because we’ve kept things fairly organized along the way, changes to our view are minimal but at the same time we’re able to improve the user experience a fair bit. We’ve added an easy way to get content into the root of our document and simplified our partial views. And look…

[![image](http://jameschambers.com/wp-content/uploads/2014/07/image_thumb3.png "image")](http://jameschambers.com/wp-content/uploads/2014/07/image13.png)

…we even managed to keep our related files in one place in the solution explorer.&nbsp; <style type="text/css">.csharpcode, .csharpcode pre
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

Our delete is now protected by the dialog, but we’d like to give the user a better picture of what it is they are deleting from the database. Tomorrow, in our last post in the series, we’ll look at wiring up the dialog box to load contents via AJAX. 