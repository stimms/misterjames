title: Setting up ASP.NET MVC 3 to work with jQuery UI 1.9
tags:
  - Asp.Net MVC
  - jQuery
  - jQuery UI
id: 931
categories:
  - Code Dive
date: 2011-10-06 18:16:00
---

> Caution: I don't recommend trying this in a production environment. The branch of jQuery UI I'm using in this series is not considered stable and is not meant for prod releases.&nbsp; You can, however, learn about the new features in the UI library and stretch your development capabilities! 

### The New and Juicy Bits

jQuery UI continues to impress. The development team's commitment to extensibility means that their own widgets are up for review at each release, and even at each milestone of a release.&nbsp; The great thing about the modularity in jQuery UI is that you can build your own widgets or customize the existing ones with the very parts used to build the core set in the first place. 

But, before you even have to venture in that direction, you already have the ability to work with the many themes (or roll your own!) to make it look and feel the way you want to.

Focusing on stability and flexibility in the 1.9 build, here are some of the important bits that will be surfacing in this release of jQuery UI:

*   Improved, tested code in many of the existing widgets  <li>Much better accessibility accross the suite (for disabled users/browsers)  <li>Revisited the API design on a number of widgets (Button, Dialog, Position, ProgressBar, Slider, Tabs)  <li>Improved collision detection on Position and added a new flipfit mode  <li>A new version of Theme Roller, the download builder and the web site  <li>New widgets: Tooltip, Spinner, Menu and Menubar 

&nbsp;

Here's the descriptions of the new Widgets from the horse's mouth:

*   **Tooltip**: A tooltip is a small overlay that sits over the main page that can be used to provide content or functionality that does not need to be always visible.  <li>**Spinner**: A spinner (stepper) is a simple widget that allows users to increment or decrement the current text box value without having to input it manually.  <li>**Menu**: Menu transform a list of anchors into a widget with mouse and keyboard support for selecting an item.  <li>**Menubar**: A horizontal menu with nested vertical flyout menus for each menu item. 

&nbsp;

### Getting jQuery UI 1.9

While I'm a huge fan of the integrated package manager in VS2010, you're not going to find these bits on Nuget.&nbsp; You're going to either need to follow the links from the [Milestone 6 post](http://blog.jqueryui.com/2011/09/jquery-ui-1-9-milestone-6-spinner-2/) or grab the bits directly from [googlecode.com](http://jquery-ui.googlecode.com/files/jquery-ui-1.9m6.zip).

Once you grab the zip, unpack it and have a look at the contents.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_6.png "image")

Pretty much everything we're going to need will be in the UI and Themes directories.&nbsp; If you're not familiar with jQuery UI, the Themes directory is where all the CSS is stored for making the entire UI has a consistant look-and-feel.&nbsp; UI is where all the code exists for all components. 

You have the choice of building up your own set of functionality for the widgets you're using by including the smaller UI libraries, or you can fire up the full kit with one library.&nbsp; This allows the bandwidth-concerned developer the opportunity to choose lighter weight code files (or even compile only the needed scripts into a minified script of their own creation).

As an example, if you wanted to make use of the new spinner, you'd need to include:

*   jquery.ui.core.js  <li>jquery.ui.widget.js  <li>jquery.ui.button.js  <li>jquery.ui.spinner.js 

Of course, you would also need to include the relevant CSS files, but with a total weight of 7Kb (and the option to use multiple CDNs out on the web) you might as well just include the full jQuery-ui.css.

### Using jQuery UI 1.9 in ASP.NET MVC

I'm going to assume you've done a little MVC work before, if not, hold tight for the walkthrough after the bullet list. I'm using version 3 of the MVC Framework in Visual Studio 2010 SP1.

Here's the steps:

1.  Create a new MVC 3 project using the "Empty" template  <li>Delete the "themes" folder in Content  <li>Update jQuery with NuGet  <li>Add jQuery UI 1.9 files to the project ("themes" go in Content and "UI" go in Scripts)  <li>Add a HomeController  <li>Add an Index view for HomeController  <li>Modify the _Layout.cshtml file (your master page) to include jQuery UI and the theme CSS 

&nbsp;

## Walking Through the Setup

Here's the visual play-by-play of that in action.

### Creating and Cleaning up the Project

Open up Visual Studio 2010 and select File –&gt; New Project.&nbsp; Create a new ASP.NET MVC 3 Application.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_3.png "image")

Name the project and click OK.&nbsp; Finally, select the “Empty” template from the list and click okay.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_thumb_2.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_8.png)

Navigate to the content folder at the root of the project and delete it.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_11.png "image")

### Updating jQuery

Next, we need to get the latest version of jQuery so that jQuery UI 1.9 is happy with us.&nbsp; Go to the Package Manager Console and type:

<span style="font-family: 'Lucida Console'" face="Lucida Console">Update-Package jQuery</span>

…and you’ll see the following:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_14.png "image")

NuGet (the heart of this console) happily goes off and updates all our dependencies for us.

### Adding jQuery UI 1.9

Pop over to that zip folder you downloaded. You’ll need to make sure you’ve extracted all the files. Copy the themes directory, then switch over to VS2010 and paste it into the content folder.&nbsp; Do the same with UI, but paste it as a sub folder into Scripts.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_20.png "image")

### Adding the Controller and View

We’re going to use the tooling in MVC 3 to do the next bit of work for us, in particular, the code generation that creates controllers and views for us.&nbsp; Right-click on the Controller folder, then select **Add –&gt; Controller**.&nbsp; Call it HomeController when the dialog prompts you (this is so that we can take advantage of the default routing rules in MVC).

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_19.png "image")

Your HomeController class is automatically generated with the Index action created for you.&nbsp; Right click on the name of this action and select **Add View**. Accept the default name (it picks Index for you because that’s the action you right-clicked).

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_23.png "image")

### Updating the Layout

Almost there!&nbsp; All that’s left to do is to adjust our Layout.&nbsp; The _Layout.cshtml file is like a MasterPage from our days of WebForms and we use it to inject our head element containing the script and CSS references. 

There’s already a jQuery reference there, so that needs to be update to the correct version (1.6.4 as at the time of this writing).&nbsp; You’ll need to add the jquery-ui.css and jquery-ui.js files as well.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_thumb_8.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Using-jQuery-UI-1.9-With-ASP.NET-MVC-3_BF3D/image_25.png)

For you copy-and-pasters out there:
<pre>&nbsp;&nbsp;&nbsp; &lt;link href="@Url.Content("~/Content/themes/base/jquery-ui.css")" rel="stylesheet" type="text/css" /&gt;&nbsp;&nbsp;&nbsp;&nbsp; &lt;script src="@Url.Content("~/Scripts/jquery-1.6.4.min.js")" type="text/javascript"&gt;&lt;/script&gt;&nbsp;&nbsp;&nbsp;&nbsp; &lt;script src="@Url.Content("~/Scripts/ui/jquery-ui.js")" type="text/javascript"&gt;&lt;/script&gt;</pre>

And that’s it! You’re all set up to use jQuery UI 1.9.

# A Convenience Disclaimer

Things are a little drawn out right now because we can't grab the preview bits on NuGet.

Keep in mind that once the updated library is released, you will simply be able to update jQuery UI through the package manager console in Visual Studio 2010\. This, in turn, will resolve and update all other dependancies and will also incorporate the vsdoc files needed for IntelliSense.

<div><span class="Apple-style-span" style="font-size: 20px; font-weight: bold">For You To Do</span></div>

Right now we really only have the guts in place to make jQuery UI tick, but it's always good to stay on top of your game.&nbsp; Get your hands dirty now!

1) Download the jQuery UI 1.9 development branch ([Milestone 6 post](http://blog.jqueryui.com/2011/09/jquery-ui-1-9-milestone-6-spinner-2/))

2) Create an ASP.NET MVC 3 project

3) Get jQuery UI 1.9 in your project and start exploring

4) Try jQuery UI 1.9 in a copy of one of your current project and see if anything breaks

# Next Up: Using the Spinner with Entity Metadata

Now that you’ve got the library in your project, we’re going to have look at some of the controls in more detail. One of the great things in MVC is the insanely simple&nbsp; rendering of templates and data binding, especially when coupled with something like Entity Framework.

Hold on to your hats, in the next segment we’re going to use data annotation attributes to wire up a jQuery UI Spinner widget.