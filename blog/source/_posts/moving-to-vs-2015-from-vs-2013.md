title: Moving to VS 2015 from VS 2013
tags:
  - ClearMeasure
  - MVC5
  - MVC6
id: 6921
categories:
  - MVC6 Conversion
date: 2015-07-23 20:45:48
---

The folks on the Visual Studio team have been making it increasingly easier to move from version-to-version with less impact on our projects. In this post I’m going to examine the process of moving from Visual Studio 2013 to Visual Studio 2015.

_In this series we’re working through the conversion of an MVC 5-based application and migrating it to MVC 6\. You can track the entire series of posts from the _[_intro page_](http://jameschambers.com/2015/07/upgrading-a-real-world-mvc-5-application-to-mvc-6/)_._

## The Backstory

First, a bit about our application. I said this was a real-world application, and it is. You can [clone the repo](https://github.com/ClearMeasureLabs/ClearMeasureBootcamp) and run it locally if you like.

![image](http://jameschambers.com/wp-content/uploads/2015/07/image10.png "image")

What we have is an expense report application, albeit a little on the light side for features. No worries, though, that is the intent! But the real pieces of an application sporting [Onion Architecture](http://jeffreypalermo.com/blog/the-onion-architecture-part-1/) are in place, and there’s quite a bit of commonly-used tech in this bad boy that you would likely find in any real-world app:

*   [![image](http://jameschambers.com/wp-content/uploads/2015/07/image_thumb4.png "image")](http://jameschambers.com/wp-content/uploads/2015/07/image11.png)Separate projects for separate concerns
*   A UI Project that stands with only a single reference (to Core)
*   An older version of NHibernate
*   Unit tests
*   HTTP Handlers
*   The Bootstrap CSS/JS library
*   StructureMap for dependency injection
*   A database migrations library
*   A custom workflow engine

So, this is no File –&gt; New –&gt; Project here, this is the real deal.

## First Steps

The first issue we’re going to address is the fact that our solution and project are currently in VS 2013 “mode”.&nbsp; In the past, you’d likely have to walk through some kind of conversion process and this was usually a compelling enough reason for people to back delay an upgrade. Did you ever have issues with incorrect file paths, incompatible project type identifiers, broken project references and builds broken due to missing dependencies? At times, I’ve even had to resort to manually editing the solution and project files to get a project back online.&nbsp; **Thankfully, this is nothing like that**. There is nothing major we need to do in order to get our project open in Visual Studio 2015, just load the solution from disk.

In the case of Bootcamp, our project opens cleanly and builds as we would expect. But you’ll notice right away a change in the repo.

![image](http://jameschambers.com/wp-content/uploads/2015/07/image12.png "image")

Visual Studio 2015 runs IIS Express in a more specific context than previous versions, which is a huge win. The applicationhost.config is basically everything you need to get the web server up locally while you develop, debug and test, and it replaces what we would used to use for IIS Express for machine-wide configuration.

This file also happens to be in the .vs directory, used only for devs running Visual Studio on Windows. It’s also easily regenerated for other developers and we don’t need to check it into source control, lest we be endlessly trumping each others’ local changes. Instead of any project or solution modifications, we’re instead going to modify the .gitignore file, adding the following line:

<pre class="csharpcode">**/src/.vs/</pre>

Great stuff! Easy, and we’re running in the latest version of everyone’s favorite IDE. We haven’t yet made any project structure changes, haven’t targeted any new .NET bits, but now we’re ready to start those pieces.

The pull request for this post can be [reviewed here](https://github.com/ClearMeasureLabs/ClearMeasureBootcamp/pull/10).

**<font style="background-color: rgb(204, 204, 204);">&nbsp; Pro tip&nbsp; </font>** To get your solution to open in Visual Studio 2015 by default, instead of Visual Studio 2013, you can simply update the first few lines of your .sln file to include the following: 
 <font face="Lucida Console"># Visual Studio 14

VisualStudioVersion = 14.0.23107.0</font>

## Next Steps

I’ve been running different incarnations of VS 2015 on my machine alongside Visual Studio 2013 (and for a period, VS 2010 as well) without any gotchas. There are some great new features that are worth checking out (it’s [a long list](https://www.visualstudio.com/en-us/news/vs2015-vs.aspx)), and your team may be able to leverage them.

While this is a short and single-focused post, I hope you see that opening the project in VS 2015 may yield no negative side effects. Heck, if you’re not sure you want to try it on your metal, you can even jump on a free trial of Azure and attempt to open your project on a VM running 2015.

Happy coding! ![Smile](http://jameschambers.com/wp-content/uploads/2015/07/wlEmoticon-smile2.png)