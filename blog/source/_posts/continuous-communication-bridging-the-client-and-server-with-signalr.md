title: 'Continuous Communication: Bridging the Client and Server with SignalR'
tags:
  - Asp.Net MVC
  - jQuery
  - SignalR
id: 841
categories:
  - Code Dive
date: 2012-02-10 18:05:00
---

**<font color="#333333"></font>**&nbsp;

**<font color="#333333">Have you ever tried to solve concurrency, or at least notification of concurrency threats? Have you worked out a way to keep data flowing between the client and the server? How about multiple client-side subscribers and the server? Server-to-client notification? And, if you have, were you able to make it easily available in other projects? Easily extensible?</font>**

If you’re a .NET developer, there’s no more guessing. SignalR is your new BFF.

It’s a new web, people.&nbsp; We all know this. We don’t make “web pages” or even “web sites” any more. We make living, breathing applications. More and more, this also means a social aspect, and increasingly this means collaboration.

## <font style="font-weight: bold">SignalR and the MVC Framework</font>

It’s my thing. I spend most of my time in Asp.Net working with the MVC Framework, so my first question is, “Does it work with MVC?”.&nbsp; Of course!

And it’s super simple:

1.  You’re going to **create a project** &amp; install SignalR via NuGet  <li>**Build your backend** by creating one class that inherits from one of the SignalR base classes  <li>Finally, you need to **wire up your view** by writing a tiny bit of script on the client 

&nbsp;

How does it work?&nbsp; Crazy ninja magic. Covered with chocolate and ice cream.&nbsp; Two scoops.&nbsp; Let’s walk through a simple, but meaningful example (you can checkout the hello world on Github) and then revisit the interesting bits.

## <font style="font-weight: bold">Creating the Project</font>

Let’s start by creating a new MVC4 project (all the kids are doing it) in Visual Studio 2010\. I’ve called mine Mvc.SignalR.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_3.png "image")

Next, we’re going to pick the empty template because we’re not too concerned about the whole flashy starter pages and default styling.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_5.png)

And we’re off to the races!&nbsp; Let’s drop into the package manager console and install SignalR. Type the following:

<font size="3" face="Lucida Console">Install-Package SignalR</font>

…and NuGet will do it’s magic for us, adding the required dependencies and updating our project accordingly.&nbsp; What else is cool there?&nbsp; Glad you asked!&nbsp; NuGet also went ahead and updated any other libraries that SignalR takes a dependency on at a higher version. Sweet! Check out your console for all the details, that’s some great stuff there.

Next, we’re going to need a controller and a view so that we have something to see.&nbsp; Right-click on your Controllers folder (in Solution Explorer) and click “Add –&gt; Controller…” and create a new “HomeController”.&nbsp; You’ll be taken to the class, at which point you can right click on the Index method and then select “Add View…”. Again, just create an empty view called Index.&nbsp; Press F5 to run the app and you should see this:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_thumb_2.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_7.png)

Not too much going on, but we’re just getting to the great stuff.&nbsp; One more quick thing, we’re going to need it later, let’s stuff the client’s IP address into the ViewBag in our Index method of the HomeController.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_17.png "image")

&nbsp;

## <font style="font-weight: bold">Building the Backend</font>

SignalR makes things super-easy for us to create the backend bits required for the continuous communication: we only need to inherit from a single class. What we’ll do in this sample is make a page visit counting mechanism that provides live updates to viewers of the page.&nbsp; Original idea, and totally worth a patent. I’ll call it, “Analytics”. On we go.

The first bit is in creating a model class to record the visit. We’ll keep it simple and track only a few properties, but we’re going to take advantage of EF Code First to use as a data store. Create two classes in your Models folder, SiteVisit and SiteVisitContext.&nbsp; Their implementations look like this:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_10.png "image")

Note that inheriting from DbContext requires importing the System.Data.Entity namespace in the code below.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_12.png)

Poof! We have a database.&nbsp; Next, I’ve got a simple class called UserLink where we provide the function that will be accessible from the client, through SignalR.&nbsp; We inherit from Hub here to give us a whole bunch of “free” client-side goodness and make sure the SignalR bits know about us. Reflection is a beautiful thing.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_thumb_4.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_14.png)

All I’m doing here is accepting an IP address, saving it to the SiteVisits table (that we created above through EF) and then getting a record count.&nbsp; The interesting bit happens after we make that result message.

Clients is of type Dynamic.&nbsp; There is no method on it called updateViewCount.&nbsp; It doesn’t really know what I want to do, other than at runtime it’s going to have to go figure out where updateViewCount is and pass it my string.&nbsp; Where does the method actually exist?&nbsp; On the client, mes amis.&nbsp; We’re writing that bad boy in JavaScript.

## <font style="font-weight: bold">Wiring up the View</font>

At this point, we need to make the client aware of the backend hub we’ve created. Open up _Layout.cshtml from Solution Explorer (it’ll be in your Views/Shared folder) and add a script reference to page. You’ll also need to update the reference to jQuery (remember how NuGet got us up-to-date?):

<font face="Lucida Console">&lt;script src="@Url.Content("~/Scripts/jquery-1.6.4.min.js")" type="text/javascript"&gt;&lt;/script&gt;
&lt;script src="@Url.Content("~/Scripts/jquery.signalR.js")" type="text/javascript"&gt;&lt;/script&gt;
&lt;script src="/signalr/hubs" type="text/javascript"&gt;&lt;/script&gt;</font>

Now, every page in our application (that uses this layout) will have access to SignalR.

“Wait a minute!”, says the observant, kung-fu yielding developer, “Script source is /signalr/hubs? But there’s no file that corresponds to that!” 

Right you are, Darryl.

SignalR does this brilliant thing: it exposes your Hub classes through a route that it creates and initializes on your behalf. Your client is then able to access the methods of your class from JavaScript. It builds the library dynamically on the first call, caches it (until your application is recycled), and then takes care of resolving the calls for you.

Great, so our _Layout is updated to include the JavaScript bits we need, and SignalR is all up in our browser like a tree fort in a…tree.&nbsp; So what do we need to do in our view?&nbsp; Well, we want to have the IP address available from script, and we need a placeholder for our messages to come back to:

<font face="Lucida Console">&lt;input type="hidden" id="client-ip" value="@ViewBag.ClientIpAddress" /&gt;
&lt;div id="view-updates"&gt;&lt;/div&gt;</font>

Good, simple stuff.&nbsp; Now the script.&nbsp; 

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_20.png "image")

I’m using jQuery (‘cause it’s awesome) to wire up an onload page handler and capturing the IP address from our hidden input (that we injected into our ViewBag in the controller). So far, so good.&nbsp; 

Next we fetch our proxy to our hub dynamically through SignalR’s connection object, then use this proxy to wire up a callback. This is the method that will be executed from our Hub.&nbsp; My handler simply takes a string and pushes it into a &lt;p&gt; tag.&nbsp; Finally, we start the connection and pass in an anonymous function to be executed once SignalR is up and running.&nbsp; This is where we make the call to our method in the hub.&nbsp; So, to recap:

1.  We create a proxy  <li>We setup an event handler  <li>We startup the connection, and when complete, call our recordView method on the hub (I’ll get back to the timing of startup in a minute) 

&nbsp;

Sweet.&nbsp; Hit F5, and see all your hard work come together…in fact, open a few browsers and watch as ALL the clients are updated when another loads.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e4133c3ea69e_C1C6/image_23.png "image")

Hit refresh on any one of those to witness the awesomeness.&nbsp; Nice work, fearless web developer!

## <font style="font-weight: bold">Things I Learned Along the Way</font>

If you start poking through the samples and venturing off to create new, more complex scenarios you may run into a couple things that aren’t at first apparent. I did, and hope that these points can help others who are looking.

*   **Case Matters**
        *   Class names and function names in c# are converted to camel case on the client side. This is important to know because, for example, if you try to get a reference to the proxy/hub for your ClassName, it would have to be $.connection.className. Conversely, client-side event handlers aren’t converted to TitleCase, so if your function is functionName, you’re going to call functionName against the dynamic Clients object in your hub.  <li>If you don’t follow casing conventions, you’ll run into errors with SignalR:
            *   “Unable to set value of the property 'someCallBack': object is null or undefined” is likely a class case issue, happens when registering a callback in a SignalR hub.  <li>“Object doesn't support property or method 'YourMethod'” is likely an issue with case on a method in your hub, happens when you try to call the method after you’ve started the hub, and the JavaScript runtime is ouching because it can’t find the method in the proxy SignalR has generated. <li>**Wait for Startup to Finish**
        *   While this seems obvious, many of the samples I found used a button click handler to make a call into the hub, which allows for extra time for the script to finish starting up. I tried at first to invoke a method right after my call to $.connection.hub.start(). I got the following error:
            *   SignalR: Connection must be started before data can be sent. Call .start() before .send() <li>If you run into this scenario just use the method I coded above.&nbsp; $.connection.hub.start() has an overload that accepts a callback as parameter, to be executed when the startup cycle is completed.&nbsp; There is also an overload that accepts settings; you can view the client API [here](https://github.com/SignalR/SignalR/wiki/SignalR-JS-Client). 

&nbsp;

## <font style="font-weight: bold">Next Steps</font>

Don’t stop here, you’ve got some ‘sperimenting to do!

1.  Make sure Visual Studio 2010 is up to date. If you don’t have a copy, get it here.  <li>Get to know your open source community projects.&nbsp; Things like SignalR are helpful extensions to your projects and make development fun.&nbsp; There are a ton of worthwhile projects that you should get to know., but reviewing the source can lead you in new directions as well.  <li>Get involved! Sure, you can use SignalR, but you can contribute as well.&nbsp; Check out the project on [GitHub](https://github.com/SignalR), or if you have questions, pop into [JabbR](http://jabbr.net/#/rooms/signalr) and connect with some of the helpful folks there. 

&nbsp;

## <font style="font-weight: bold">A Quick Thanks</font>

I want to sent a quick thanks out to [Michael Carter](http://twitter.com/kiliman) who helped through some timing and casing errors on JabbR while I put this sample together, David Fowler for sharing his excellent work on this project (the guy is absolutely brilliant) as well as [Steve Lydford](https://twitter.com/#!/stevelydford) for the docs/samples he contributed to the project.