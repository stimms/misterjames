title: 'Day 28: Doing More Interesting Things With Buttons'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4591
categories:
  - Code Dive
date: 2014-07-18 22:25:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Using the styling of Bootstrap or a replacement theme is a great way to make your site stand out, and buttons are a great example of making the UI more rich and friendly to users. They are likely the first thing you’d write some CSS for if you were starting from scratch on your site, so the tweaks and highlights they offer out-of-box are quite welcome.

## Basic Buttons

If you don’t style your buttons, they’re going to kinda suck. Well…it’s not that they won’t work or anything, but you will just get the boring old browser-styled version of a button.

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image6.png "image")

And because each browser has it’s own default stylesheet, you can’t be guaranteed that any user will see the same font, layout or spacing. Thankfully, enforcing these attributes across clients is only an attribute away.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-primary btn-sm"</span><span class="kwrd">&gt;</span>A button<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-success btn-sm"</span><span class="kwrd">&gt;</span>Another button<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
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

You’ll notice that I used the btn-sm class in the source above, there are large (btn-lg), regular (no class needed), small (btn-sm) and extra-small (btn-xs) to choose from. And now my buttons look like so, in every browser:

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image7.png "image")

If I want them to be the same width, I can also use the grid system for sizing, dropping, say, a col-md-4 into the class attribute to get the following result:

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image8.png "image")

## Grouping Things Together

The buttons look better now, but they’re bumping up against each other. They’d look better still if they looked like they were part of the same control group. We can assemble buttons into groups by providing a touch more markup to the elements, as seen below, with a simple DIV wrapper.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="btn-group"</span><span class="kwrd">&gt;</span>
    <span class="rem">&lt;!-- buttons here --&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
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

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image9.png "image")

I changed the text here because this is the code that I’ll be working into my partial for the notifications.&nbsp; I also made the buttons the same color – from a design stand point I think it looks better (and, heck, I’m even color blind) – and I removed the grid sizing on the buttons to allow them to group properly without having to add custom CSS styles. 

## Putting Some Fancy On

Chances are we could get away here with the two buttons side-by-each, but in the event that we need to add more actions the page is going to start to get a little wide. An improvement we can make here is to give the users a default action, but then to put the rest of the less-commonly accessed actions into a dropdown to look like this:

![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image10.png "image")

This is called a split button dropdown, where a clickable action is available and additional commands are tucked into the drop down. Rather than wrapping the buttons in just a DIV, we also introduce a caret for the dropdown and have a UL to act as a container for the non-visible actions.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="btn-group"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-success btn-sm"</span><span class="kwrd">&gt;</span>Mark as Read<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-success btn-sm dropdown-toggle"</span> <span class="attr">data-toggle</span><span class="kwrd">="dropdown"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="caret"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="sr-only"</span><span class="kwrd">&gt;</span>Toggle Dropdown<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="dropdown-menu"</span> <span class="attr">role</span><span class="kwrd">="menu"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Delete<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Send SMS<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Make cheese<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span> <span class="attr">class</span><span class="kwrd">="divider"</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Baz<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
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

I’ve added in the correct attributes on the caret button element – namely the data toggle – so that it plays properly with the framework. Per [the documentation](http://getbootstrap.com/getting-started/#accessibility), I’ve also added an sr-only class that allows screen readers to make sense of the control grouping. The inner commands are now simply anchor tags and don’t require any of the button classes.

So, there all-of-a-sudden seems to be a lot going on there, but let’s break down the basic structure:

*   A DIV to group everything together
*   A BUTTON for the main action
*   A BUTTON for the caret (dropdown)
*   A UL with a list of LIs for the additional commands.

The skeleton looks like this:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="btn-group"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn"</span><span class="kwrd">&gt;</span>Primary Action<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn dropdown-toggle"</span> <span class="attr">data-toggle</span><span class="kwrd">="dropdown"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="caret"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="sr-only"</span><span class="kwrd">&gt;</span>Toggle Dropdown<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="dropdown-menu"</span> <span class="attr">role</span><span class="kwrd">="menu"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>...<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>...<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>
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

One final note: You would correctly assume, were you the assuming type, that the drop down functionality requires the JS library to work (which is already included in our web site bundle and layout).

## Updating our View

Now, forget what we’ve done so far in the TD for our actions. We’re going to change it up a bit and get things properly in line for some real-world action.

Since we’re going to be changing data we are going to want to do a POST back to the server, so we have the option of using the Html.BeginForm helper.&nbsp; But generating a series of forms representing all actions for all notifications could get complicated quite quickly (in other words, we don’t want a form for each action, for each notification). Instead, we need to decorate our split buttons with the appropriate IDs, then put together a bit of JavaScript to do the submit for us on a single, static form. The form contains a hidden input for the ID and will be at the end of the document in _RenderNotifications, followed by the related JavaScript:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">form</span> <span class="attr">id</span><span class="kwrd">="notificationForm"</span> <span class="attr">method</span><span class="kwrd">="POST"</span><span class="kwrd">&gt;&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="hidden"</span> <span class="attr">name</span><span class="kwrd">="id"</span> <span class="attr">id</span><span class="kwrd">="notificationFormItemId"</span> <span class="kwrd">/&gt;&lt;/</span><span class="html">form</span><span class="kwrd">&gt;</span>

<span class="kwrd">&lt;</span><span class="html">script</span> <span class="attr">type</span><span class="kwrd">="text/javascript"</span><span class="kwrd">&gt;</span>

    <span class="kwrd">var</span> readUrl = <span class="str">'@Url.Action("MarkNotificationAsRead")'</span>;
    <span class="kwrd">var</span> deleteUrl = <span class="str">'@Url.Action("Delete")'</span>;

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

<span class="kwrd">&lt;/</span><span class="html">script</span><span class="kwrd">&gt;</span></pre>
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

Here we are using the URL helper method to resolve the URLs needed for read and delete actions.&nbsp; The JavaScript simply accepts the ID and an action parameter, then sets up the form correctly and submits it. 

Next, skip back up the document a little to the TD that contains our split button. We’ll replace the entire contents of the TD with the following:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="btn-group"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">a</span> <span class="attr">class</span><span class="kwrd">="btn btn-success btn-sm"</span> <span class="attr">href</span><span class="kwrd">="javascript:updateNotification(@notification.NotificationId, 'read')"</span><span class="kwrd">&gt;</span>Mark as Read<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-success btn-sm dropdown-toggle"</span> <span class="attr">data-toggle</span><span class="kwrd">="dropdown"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="caret"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="sr-only"</span><span class="kwrd">&gt;</span>Toggle Dropdown<span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="dropdown-menu"</span> <span class="attr">role</span><span class="kwrd">="menu"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="javascript:updateNotification(@notification.NotificationId, 'delete')"</span><span class="kwrd">&gt;</span>Delete<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Send SMS<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Make cheese<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span> <span class="attr">class</span><span class="kwrd">="divider"</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#"</span><span class="kwrd">&gt;</span>Baz<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>
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

You’ll notice that our A tags are now just invoking our JavaScript method, passing in the notification ID of the current record (remember that we’re in that outer foreach loop here). We can continue to develop new methods by wiring up the A tag and extending the switch statement should we like.

## A Couple of Controller Methods

Finally we’re going to need to wire up our controller to catch those user actions and do the dirty bits. Marking the notification as viewed will be first. We’ll use the DB context to retrieve, update and save the selected record, so we’ll make a small change to the way we initiate the DB context and move it to a class-level declaration as such: 
<pre class="csharpcode"><span class="kwrd">private</span> <span class="kwrd">readonly</span> SiteDataContext _context = <span class="kwrd">new</span> SiteDataContext();</pre>
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

Remember to remove the following line of code from the GetUserNotifications method and update all occurrences of context to _context (your code won’t compile if you don’t).
<pre class="csharpcode">[HttpPost]
<span class="kwrd">public</span> ActionResult MarkNotificationAsRead(<span class="kwrd">int</span> id)
{
    var userNotification = GetUserNotifications().FirstOrDefault(n=&gt;n.NotificationId == id);

    <span class="kwrd">if</span> (userNotification == <span class="kwrd">null</span>)
    {
        <span class="kwrd">return</span> <span class="kwrd">new</span> HttpNotFoundResult();
    }

    userNotification.IsDismissed = <span class="kwrd">true</span>;
    _context.SaveChanges();

    <span class="kwrd">return</span> RedirectToAction(<span class="str">"Manage"</span>);
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

Similarly, the code for the delete action will use the same DB context to delete the requested record.
<pre class="csharpcode">[HttpPost]
<span class="kwrd">public</span> ActionResult Delete(<span class="kwrd">int</span> id)
{
    var userNotification = GetUserNotifications().FirstOrDefault(n =&gt; n.NotificationId == id);

    <span class="kwrd">if</span> (userNotification == <span class="kwrd">null</span>)
    {
        <span class="kwrd">return</span> <span class="kwrd">new</span> HttpNotFoundResult();
    }

    _context.Notifications.Remove(userNotification);
    _context.SaveChanges();

    <span class="kwrd">return</span> RedirectToAction(<span class="str">"Manage"</span>);
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

There are a couple of quick notes here that I should mention. From a security standpoint we’re doing some good things, namely, we’re using a common method to only load into scope the notifications of the currently logged in user. We’re leveraging the fact that the framework provides a mechanism to correctly provide the user’s identity and limiting the user’s request to only allow execution of modifying or deleting records in a “row-level” security kind of way. Our controller is marked with the Authorize attribute, so if we couple this with SSL we’d be in pretty good shape.

Then there’s the bits that aren’t really good practice, but are good enough for the point of this exercise. First, we’ve got this GetUserNotifications method on the AccountController class.&nbsp; What about “AccountController” says anything about “loading the notifications for a specific user”? Nothing, right? This data access, as I’ve mentioned before, should be pushed into a repository that makes use of the DB context through a Dependency Injection mechanism that only creates one instance of the DB context per request. And, the controller should take that repository through DI.&nbsp; This would make things more flexible (why write data access in more than one spot) and testable (we could use mocks to test more easily).

## Next Steps

Certainly you wouldn’t want your users deleting data without confirmation (if at all), so in the next installment we’ll have a look at putting a dialog in place to ensure users are invoking the intended action.