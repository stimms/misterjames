title: 'Day 24: Storing User Profile Information'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 4301
categories:
  - Code Dive
date: 2014-06-30 10:42:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Session is an attractive and oft-used mechanism for storing user profile data. It’s freely available and has been around forever. But session state and logged-in user identity, while they may seem closely related, do not operate on the same lifecycle and sensitive data, personal data, could get “leaked out”.

And hey…this post assumes you’ve been following along with the series and you have already signed in at least once with a registered user.

## A Better Place to Store User Profile Data

In [yesterday’s article](http://jameschambers.com/2014/06/day-23-choosing-your-own-look-and-feel/) we penned in the user’s choice of Bootstrap Theme and stored their selection in the Session object. Brock Allen wrote a great post a couple years back on why this kind of thing isn’t a great approach. The things that concern us most are that:

*   By default, session isn’t durable and doesn’t scale  <li>Even if we move to a more durable session management approach, there’s no persistence of session data and yet we’re making network hops  <li>Session isn’t tied to users signing in or out 

So, today, we’re going to fix that Session access with a permanent, more reliable and secure approach.

## Extending the User Profile Data

Since you’ve already [created an account](http://jameschambers.com/2014/06/day-22-sprucing-up-identity-for-logged-in-users/), you technically have some profile information already stored in your site’s database. We’re going to leverage the identity infrastructure of our project and extend the class that keeps track of our data, adding in the user’s selected theme. In your Solution Explorer, locate the ApplicationUser class under Models. The easiest way is usually just through the search box at the top:

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb21.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image42.png)

Add the following property to your ApplicationUser class:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">string</span> CssTheme { get; set; }</pre>
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

Something to keep in mind: we’re going to be modifying a class that participates in Entity Framework persistence, as managed by the Asp.Net Identity libraries. This is handy, but not free. Because we’re changing essentially the schema of a table, we’ll also need to enable and create a migration from the Package Manager Console:
<pre class="csharpcode">Enable-Migrations -ContextTypeName SimpleSite.Models.ApplicationDbContext -MigrationsDirectory Migrations\Identity
Add-Migration CssTheme -ConfigurationTypeName SimpleSite.Migrations.Identity.Configuration</pre>
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

Note that we’ve got a pre-existing migration on our site, **so we need to now be more specific** and explicitly name the Configuration type. Also, in enabling the migrations, I added the MigrationsDirectory folder for the DbContext, so that our identity-related migrations would be in a sub-folder. 

The Entity Framework created the appropriate classes for me to track DbContext-specific settings and the migration that I needed to update the database, but those are just the classes, nothing’s changed in the DB yet. That all needs to be followed by an update to our database like so:
<pre class="csharpcode">Update-Database -ConfigurationTypeName SimpleSite.Migrations.Identity.Configuration</pre>
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

We can now store the data we need to, we just need a way to be able to stuff it in there.

## Updating our Controller

Since we’re now targeting a user managed from the Identity libraries we’re going to wipe out the code that writes to session and instead use the relevant classes to locate the user profile and update it. Head over to ProfileController.cs (in Helpers\) and replace the call to Session with the following:
<pre class="csharpcode">var userStore = <span class="kwrd">new</span> UserStore&lt;ApplicationUser&gt;(<span class="kwrd">new</span> ApplicationDbContext());
var manager = <span class="kwrd">new</span> UserManager&lt;ApplicationUser&gt;(userStore);
var user = manager.FindById(User.Identity.GetUserId());
user.CssTheme = themename;
manager.Update(user);</pre>
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

We resolve the user through a UserManager, and the manager needs a UserStore to locate it’s data. We can then set the CssTheme and update the user through the manager. ApplicationUser and ApplicationDbContext are just the names of the types that were automatically created for you through the project template.

It’s only 5 lines of code to access and update a property, but you can see how there must be a better solution to all this than having to write this code all the time. Some day I bet a post will pop up on that…

## Updating our Layout

Open up _Layout.cshtml (in Shared\Views) and update the code that resolves the correct style bundle. We’re going to use a similar approach to what we did in the controller to find the user and property, and we’ll handle the null case a little differently than we were previously as well by setting the default in advance. 
<pre class="csharpcode">@{
    var theme = Bootstrap.Theme.Stock;
    <span class="kwrd">if</span> (User.Identity.IsAuthenticated)
    {
        var userStore = <span class="kwrd">new</span> UserStore&lt;ApplicationUser&gt;(<span class="kwrd">new</span> ApplicationDbContext());
        var manager = <span class="kwrd">new</span> UserManager&lt;ApplicationUser&gt;(userStore);
        var user = manager.FindById(User.Identity.GetUserId());
        theme = user.CssTheme ?? theme;
    }
    @Styles.Render(Bootstrap.Bundle(theme))
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

Our other line of code – to actually render the bundle – will remain the same, as you can see at the bottom.

## Wait, What Did We Actually Do Again?

I know, I know, it’s hard to put time into something when it seems that nothing really changed. Our functionality is identical and users still have the same options as they did before.

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb22.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image43.png)

But technically our solution is a little more sound.&nbsp; Here’s what we did again as a bit of a recap:

*   We added a CssTheme property to our ApplicationUser class
*   We enabled migrations on our ApplicationDbContext, using a specify folder for our identity migrations
*   We added a migration to accommodate our new CssTheme property
*   We updated our controller to use the Identity objects instead of session
*   We refactored our _Layout to get the theme of choice from the user’s profile information

## Next Steps

For brownie points here, one could likely go to the “manage” bits (under Controllers\AccountController and Views\Account) and allow the user to make a theme selection from there. It’s likely a better home than continuously being available from the home page.

Hang on a sec, though…if we’ve got a place to store information about _specific_ users now, why don’t we revisit our code that puts up those lovely notification icons in the navbar?