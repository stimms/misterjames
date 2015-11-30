title: 'Day 26: Bootstrap Tabs for Managing Accounts'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4421
categories:
  - Code Dive
date: 2014-07-03 00:40:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

We’ve got this [notification thing](http://jameschambers.com/2014/07/day-25-personalizing-notifications-bootstrap-tables/) going on now and we’d like to give users a way to review notifications. There’s a fairly acceptable landing spot on the “Manage Account” view (at /Account/Manage in your browser), at least for the purpose of these exercises, so we’ll flesh things out there.

However, the view is isn’t really set up for notifications (it’s truthfully not the best spot) so we’ll need to give us some UI to make it work.

## Understanding the Tab Component

There are two main elements you’ll need to get the tabs going correctly, a UL tag that will set up as the menu elements, and a DIV to act as a container for the content.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="nav nav-tabs"</span> <span class="attr">role</span><span class="kwrd">="tablist"</span> <span class="attr">id</span><span class="kwrd">="accountTab"</span><span class="kwrd">&gt;</span>
  <span class="rem">&lt;!-- content --&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-content"</span><span class="kwrd">&gt;</span>
  <span class="rem">&lt;!-- content --&gt;</span>
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

Visually, you can link those elements as illustrated below:

[![image](https://jcblogimages.blob.core.windows.net/img/2014/07/image_thumb.png "image")](https://jcblogimages.blob.core.windows.net/img/2014/07/image3.png)

Because our site includes the JavaScript library for Bootstrap our tabs will automatically render and behave correctly. The classes help with the visuals, and the JS takes care of the behavior.

The UL will contain LI elements for each tab that you wish to display on the page. For us, that will the notifications, linked accounts and password reset. Those last two are the content that already exists on the page at \Views\Account\Manage.cshtml, and the notifications bits are what we’ll fill in after our tabs are in place.

In addition to those two root elements, you can use a bit of JavaScript to manipulate the tabs if needed. For example, if you wanted a particular tab displayed on page load, you could use the ID as part of a jQuery selector and call the show method as follows:
<pre class="csharpcode">$(<span class="str">'#accountTab a[href="#linkedAccounts"]'</span>).tab(<span class="str">'show'</span>);</pre>
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

The DIV for content will in turn hold container elements for the rest of the content you want on the page. The structure will look something like this:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-content"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-pane active"</span> <span class="attr">id</span><span class="kwrd">="notifications"</span><span class="kwrd">&gt;</span>
        <span class="rem">&lt;!-- content --&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-pane"</span> <span class="attr">id</span><span class="kwrd">="linkedAccounts"</span><span class="kwrd">&gt;</span>
        <span class="rem">&lt;!-- content --&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-pane"</span> <span class="attr">id</span><span class="kwrd">="passwordReset"</span><span class="kwrd">&gt;</span>
        <span class="rem">&lt;!-- content --&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>

Each of the tab-pane DIVs could also have a fade class applied, which creates a nice content-switching visual. Let’s use that.

## Updating the View

If you haven’t done so already, open up the \Views\Account\Manage.cshtml file and start cutting it up! Inside the DIV.row DIV.col-md-12 structure, add the UL for the tab headers, and add a DIV to contain the tab pages including a placeholder for notifications, and the DIVs for linked accounts and password reset. Move the content from those parts of the page in.

The final page code should be similar to the following:
<pre class="csharpcode">@using SimpleSite.Models;
@using Microsoft.AspNet.Identity;
@{
    ViewBag.Title = "Manage Account";
}

<span class="kwrd">&lt;</span><span class="html">h2</span><span class="kwrd">&gt;</span>@ViewBag.Title.<span class="kwrd">&lt;/</span><span class="html">h2</span><span class="kwrd">&gt;</span>

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="row"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="col-md-12"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">p</span> <span class="attr">class</span><span class="kwrd">="text-success"</span><span class="kwrd">&gt;</span>@ViewBag.StatusMessage<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>

        <span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">class</span><span class="kwrd">="nav nav-tabs"</span> <span class="attr">role</span><span class="kwrd">="tablist"</span> <span class="attr">id</span><span class="kwrd">="accountTab"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">li</span> <span class="attr">class</span><span class="kwrd">="active"</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#notifications"</span> <span class="attr">role</span><span class="kwrd">="tab"</span> <span class="attr">data-toggle</span><span class="kwrd">="tab"</span><span class="kwrd">&gt;</span>Notifications<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#linkedAccounts"</span> <span class="attr">role</span><span class="kwrd">="tab"</span> <span class="attr">data-toggle</span><span class="kwrd">="tab"</span><span class="kwrd">&gt;</span>Linked Accounts<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">li</span><span class="kwrd">&gt;&lt;</span><span class="html">a</span> <span class="attr">href</span><span class="kwrd">="#passwordReset"</span> <span class="attr">role</span><span class="kwrd">="tab"</span> <span class="attr">data-toggle</span><span class="kwrd">="tab"</span><span class="kwrd">&gt;</span>Password Reset<span class="kwrd">&lt;/</span><span class="html">a</span><span class="kwrd">&gt;&lt;/</span><span class="html">li</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>

        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-content"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-pane fade active"</span> <span class="attr">id</span><span class="kwrd">="notifications"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>Here's where we'll put our notifications.<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-pane fade"</span> <span class="attr">id</span><span class="kwrd">="linkedAccounts"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">section</span> <span class="attr">id</span><span class="kwrd">="externalLogins"</span><span class="kwrd">&gt;</span>
                    @Html.Action("RemoveAccountList")
                    @Html.Partial("_ExternalLoginsListPartial", new ExternalLoginListViewModel { Action = "LinkLogin", ReturnUrl = ViewBag.ReturnUrl })
                <span class="kwrd">&lt;/</span><span class="html">section</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="tab-pane fade"</span> <span class="attr">id</span><span class="kwrd">="passwordReset"</span><span class="kwrd">&gt;</span>
                @if (ViewBag.HasLocalPassword)
                {
                    @Html.Partial("_ChangePasswordPartial")
                }
                else
                {
                    @Html.Partial("_SetPasswordPartial")
                }
            <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>

@section Scripts { @Scripts.Render("~/bundles/jqueryval") }</pre>
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

All that’s left now is to get our list of user-specific notifications into our view. Tomorrow we’ll get a view model figured out, populate it with the user’s notifications and get it rendering in a Bootstrap-styled table in the view.