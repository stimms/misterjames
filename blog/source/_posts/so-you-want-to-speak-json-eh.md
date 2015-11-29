title: 'So, You Want to Speak JSON, eh?'
tags:
  - JSON
  - Windows Phone
id: 871
categories:
  - Code Dive
date: 2011-12-21 18:09:00
---

I am actively developing on ASP.NET MVC 3 and that has proven very useful to me as I make the switch to developing for Windows Phone 7.

_What?_&nbsp; you say _That doesn't make any sense!_

To which I reply: embrace the cloud, homeboy.&nbsp; 

## Okay, Wait a Minute

I know, I know, I just went from JSON to MVC 3 to Windows Phone 7 to the cloud. But, here's the thing: they're all complimentary. 

If you're looking for the code that makes returning JSON pretty much a freebie, skip ahead. <font face="Lucida Console">Else { BuckleUp(); }</font>

ASP.NET MVC is great for working with JSON – it supports binding JSON to your model classes natively, even when you're dealing with complex mappings like collections of entities nested in other entities.&nbsp; It's also trivial to return JSON encoded data from your model classes and the result types are native to the framework.&nbsp; It gets even better with LINQ to JSON and the [Json.Net](http://json.codeplex.com/) serialization library available on [NuGet](http://nuget.org/packages/Newtonsoft.Json).

JSON is lightweight, so it makes it a great candidate for pushing data around in mobile scenarios.&nbsp; If you want to work with anything other than Windows Phone 7 (what? why would you do that?!), then the freebies in the .NET world don't play so nicely and you'll want to jump on the JSON bandwagon.&nbsp; Json.Net works on WP7 as well, so now you're covered for your soon-to-be-awesome Windows Phone app.&nbsp; (By the way...go [join the movement](http://www.developermovement.com/) and get a free Xbox, WP7, $500 and more [here](http://www.developermovement.com/), if you're Canadian...publish some apps!).

Where does the cloud come into this? Thought you'd never ask.&nbsp; A refresh on the Azure toolkit earlier this year gave us MVC3 deployment templates, so pushing your app out on the web [is simple](http://www.asp.net/mvc/overview/deployment/walkthrough-hosting-an-aspnet-mvc-application-on-windows-azure). Find out more [http://blogs.msdn.com/b/cdndevs/p/canadadoesazure.aspx](http://blogs.msdn.com/b/cdndevs/p/canadadoesazure.aspx).

## Speaking JSON

So, you're doing some MVC 3 dev, you (maybe) are hosting out in the cloud, and you want to knock off some low-hanging fruit to turn your controllers and actions into full-fledged JSON participants?&nbsp; I was at a conference recently and saw how easy this was in Ruby on Rails, and figured there had to be a way to do this in MVC.&nbsp; I didn't want to have to write new versions of all my actions just to respond with the _same_ data as JSON.

Turns out this is too easy, but first we need to understand a little bit about how controllers and actions work.

Incoming requests are handled by the routing framework for us.&nbsp; A request coming in for Home/Index results in an instance of the HomeController getting created and then our Index action (method) being invoked.&nbsp; But did you know that you have the chance to use ActionFilters?&nbsp; If we create our own ActionFilter, we get the chance to modify the parameters coming into the action, we get access to the controller, the HttpContext and more.&nbsp; 

And that's just on the front end of execution. We get a chance to look at everything again – including the view data – on the back end of execution as well, meaning we can swap out our own result object.&nbsp; Huzzah! We have a vector!

## Using the Vector

Here's the plan:

*   Let the action execute  <li>Check to see if the requesting party was looking for a JSON response, and, if they were,  <li>Create a new JsonResult and push the model data from the action into it 

&nbsp;

Seems simple enough, right? Here's what the implementation looks like (and honestly, there's as much commenting in there as code):
<pre>namespace Json.ActionFilter
{&nbsp;&nbsp;&nbsp;&nbsp; public class SupportsJson : ActionFilterAttribute&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // we will use the framework default but allow users to override in the constructor&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; private JsonRequestBehavior _jsonRequestBehavior = JsonRequestBehavior.DenyGet;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; public SupportsJson() {}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // if you want to allow GET (lame!) then be my guest...&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; public SupportsJson(JsonRequestBehavior behavior)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; _jsonRequestBehavior = behavior;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // this happens AFTER the action is executed and BEFORE the result is returned&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // so it's the perfect place to swap out the result object for our JSON one&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; public override void OnActionExecuted(ActionExecutedContext filterContext)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // capture the application types&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; var applicationTypes = (filterContext.HttpContext.Request.AcceptTypes ?? new string[] {""});&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // check to see if json is in there&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (applicationTypes.Contains("application/json"))&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // swap out the result if they requested Json&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; var model = filterContext.Controller.ViewData.Model;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; filterContext.Result = new JsonResult { Data = model, JsonRequestBehavior = _jsonRequestBehavior };&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }&nbsp;&nbsp;&nbsp;&nbsp; }
}</pre>

Bam! And just like that we have us some JSON capabilities!

## Using the ActionFilter

Here's an existing ActionResult I have:
<pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; public ActionResult FooCookies(string term)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; PeopleRepository repository = new PeopleRepository();&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; var people = repository.GetPeople(term);&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return View(people);&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</pre>

How much work is it to have it return JSON instead?
<pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [SupportsJson]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; public ActionResult FooCookies(string term)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; PeopleRepository repository = new PeopleRepository();&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; var people = repository.GetPeople(term);&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return View(people);&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</pre>

Tada! That's right, just add the attribute <font face="Lucida Console">SupportsJson</font> and instantly you can repurpose any action to return JSON from the controller.

## Next Steps

Hey look, you don't need to be building Windows Phone 7 apps on Azure to take advantage of this.&nbsp; If you're building any site using jQuery or jQuery UI, if you work with other mobile platforms or even building for Windows 8, now is a good time to make sure that your data (and complex entity collections) are JSONable. So, you can start by:

*   <font size="4">Learning more about </font>[<font size="4">ASP.NET MVC</font>](http://www.asp.net/mvc)<li><font size="4">Experimenting with </font>[<font size="4">jQuery</font>](http://jquery.com/)<font size="4"> and </font>[<font size="4">jQuery UI</font>](http://www.jqueryui.com)<li><font size="4">Check out Json.Net on </font>[<font size="4">CodePlex</font>](http://json.codeplex.com/)<font size="4"> and </font>[<font size="4">NuGet</font>](http://nuget.org/packages/Newtonsoft.Json)<li><font size="4">Trying out the </font>[<font size="4">Windows Phone 7</font>](http://create.msdn.com/en-ca/)<font size="4"> or </font>[<font size="4">Azure</font>](http://blogs.msdn.com/b/cdndevs/p/canadadoesazure.aspx)<font size="4"> toolkits</font>

Good luck!