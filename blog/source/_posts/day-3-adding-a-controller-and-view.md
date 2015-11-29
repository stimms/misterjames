title: 'Day 3: Adding a Controller and View'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3011
categories:
  - Code Dive
date: 2014-06-03 10:04:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

When we talk about “adding a page” to a site, what we usually are referring to is setting up some kind of response to a client request. Sometimes that request will be an HTML page, but it might be a dynamically created image, a file built on the fly, or any other HTTP compliant result.

In any case, if the goal is to try to render something that the user is going to see on their screen, we’re likely talking about adding controllers and views.

## Creating Controllers

As we already discussed, we will follow the conventions that have been laid out so that we can leverage the built in tooling. To get started, right-click on the Controllers folder in the solution explorer and follow the context menu to “Add –&gt; Controller…”.&nbsp; This is the process for using the scaffolding exposed by the framework, launching a dialog that prompts you for the information it needs to build out a starting point. There are a number of options, but let’s just look at first one for now: the Empty Controller.

[![image_thumb[4]](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb4_thumb.png "image_thumb[4]")](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb4.png)

This gives you a simple option to name a controller when selected.

[![image_thumb[7]](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb7_thumb.png "image_thumb[7]")](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb7.png)

I called mine “SimpleController”, which you’ll see has some significance in a moment.&nbsp; As a class, future James will know exactly what present James means by this name.&nbsp; Here’s the class that is generated for me.
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> SimpleController : Controller
{
    <span class="rem">// GET: Simple</span>
    <span class="kwrd">public</span> ActionResult Index()
    {
        <span class="kwrd">return</span> View();
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

So, again, this is a class with a method, but we call it a controller with an action. In this case, my Index “action” returns the result of a call to the **View** method, which is found on my base class that I inherit from (Controller). **ActionResult,** the return type of **Index**, is a type located in the MVC Framework that we’ll look Because we’re following convention, the **View** method will attempt to locate a view named “Index” found in the Views folder, in the Simple subfolder.

Unfortunately, that view doesn’t yet exist. Thankfully, this isn’t hard to do.

## Creating Views

In the code editor window, right-click on the Index method (right on the name of the method itself) to invoke the context menu. Select “Add View…” to get the dialog open to create your view.

[![image_thumb[10]](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb10_thumb.png "image_thumb[10]")](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb10.png)

The nice thing here is that you don’t have to type in the name of your view. The tooling just assumes the name from the method.

At this point, use the “Empty” template to create your view and select the option to “Use a layout page” as I have above. Leave the name as-is so that we can follow along on that convention gravy train. When you click “Add”, a view file will be created for you with the cshtml (or vbhtml) extension, with something similar to the following code:
<pre class="csharpcode">@{
    ViewBag.Title = <span class="str">"Index"</span>;
}

&lt;h2&gt;Index&lt;/h2&gt;</pre>
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

The @{ … } notation is Razor syntax to say, “here’s a code block”. This is used to interact with the rendering of the view and in this case it’s simply setting the value of the page Title in the ViewBag. 

With your cursor in the Razor editor, run your application by pressing CTRL+F5 (run without debugger), and navigate to your newly created page. For me, my application started up on port 48995 and the full URL was <font color="#0000ff">**[http://localhost:48995/Simple/Index](http://localhost:48995/Simple/Index)**</font>. You can think of that address as <font color="#0000ff">**http://host/Controller/View**</font> for any page that is following the default convention for routing.

[![image_thumb[15]](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb15_thumb.png "image_thumb[15]")](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb15.png)

New page for the win!

## Next Steps

A page that says index will really only hold you over for so long, so up next we’ll find out how to send along more meaningful information to our client.

Happy coding! ![Smile](http://jameschambers.com/wp-content/uploads/2014/06/wlEmoticon-smile.png)