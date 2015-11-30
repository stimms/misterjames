title: Working With IAuthenticationFilter in the MVC 5 Framework
tags:
  - Asp.Net MVC
  - Visual Studio 2013
id: 2091
categories:
  - Code Dive
date: 2013-11-19 23:36:24
---

> When implementing a custom authentication filter it’s important to know where in the pipeline your filter is invoked; if your purpose is to prevent unauthorized access to a controller action, be sure to implement your credentials verification early enough in the process. This post walks you through the creation of a basic authentication filter and shows the correct method in which to do the check. Good news: it’s easy! 

We can learn a lot about the new IAuthenticationFilter interface by implementing one and seeing where it fits in the MVC pipeline.&nbsp; Before we get started, let’s first remember that authentication and authorization are separate concerns in your application, so this filter is a welcome little addition. 

Authentication is where a user provides credentials to access a resource, whereas authorization allows access to particular resources based on properties of the user’s identity. In previous versions of the MVC Framework we had the AuthorizeAttribute, which could be used to cause a redirect if you were unauthenticated, but it’s also true that it blurred the lines a little around auth &amp; auth.&nbsp; 

## Creating a Basic Do-Nothing Filter

Now that Authentication can more easily stand alone, the process to implement it is pretty simple:

*   Add a class to your project called NewAuthenticationFilter*   Inherit from ActionFilterAttribute and IAuthenticationFilter
*   Implement the interface (click on the interface name and press Ctrl+. and hit enter)
*   Throw a couple of debug statements in so that we can set breakpoints, remembering to add the diagnostics namespace to your using statements 

Just a few quick side notes on the above code. 

*   I usually stick my filters into a Filters folder in my project (or into a separate DLL project to keep them reusable) to try to keep things clean
*   We inherit from ActionFilterAttribute so that we can use it as an attribute on our actions; just using IAuthenticationFilter isn’t enough
*   You can implement interface members by clicking on the interface name and pressing Ctrl+. and then tapping Enter 

Your final class should look like the following:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> NewAuthenticationFilter: ActionFilterAttribute, IAuthenticationFilter
{
    <span class="kwrd">public</span> <span class="kwrd">void</span> OnAuthentication(AuthenticationContext filterContext)
    {
        Debug.WriteLine(<span class="str">"OnAuthentication"</span>);
    }

    <span class="kwrd">public</span> <span class="kwrd">void</span> OnAuthenticationChallenge(AuthenticationChallengeContext filterContext)
    {
        Debug.WriteLine(<span class="str">"OnAuthenticationChallenge"</span>);
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

With that in place, go the the Index action on the HomeController and add the attribute to the method. This will let the events fire as the request is processed. Also, set a breakpoint on the return statement. Your action should look like this:
<pre class="csharpcode">[NewAuthenticationFilter]
<span class="kwrd">public</span> ActionResult Index()
{
    <span class="kwrd">return</span> View();
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

Great! Let's hit F5 and see what happens:

*   The OnAuthentication method is invoked, press F5 to continue
*   The action is executed, press F5 to continue
*   The OnAuthenticationChallenge is called last

Perfect, some of how we can use this is now becoming apparent. First of all, it's important to note that the action can be fully executed before the OnAuthenticationChallenge is executed. This means that if you're intending on preventing any DB writes or calls to something mutable or executable during the action, you'll need to implement logic in OnAuthentication method to prevent this (we’ll get to that in a minute).

Second, both methods allow you to evaluate request information and set properties of the response, such as the Result (an ActionResult), but remember that the Action itself has the ability to set some of these properties and might overwrite what you set up in OnAuthentication (which, again, is called before the index action method in our example).

Finally, remember that this is just one part of the pipeline. The Result of the action hasn't yet been executed when OnAuthenticationChallenge is complete, so something like a View hasn't been rendered, or a FileResult that would load data from disk hasn't yet been called.

## But Wait! What is This All For?

I’m so glad you asked. Here’s the thing…Debug statements aren’t that interesting and don’t really show you how this can come into context. The user passes through the above code and our action is executed, as is our result, so the user gets to see whatever they requested come back in their browser. Let’s update our code and take another look at how our application might use this.

An authentication filter doesn’t really do much for us if it’s not, oh, say, _filtering for authentication_, so let’s start by checking to see if our user is actually presenting some kind of credentials. 
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">void</span> OnAuthentication(AuthenticationContext filterContext)
{
    <span class="kwrd">if</span> (!filterContext.Principal.Identity.IsAuthenticated)
        filterContext.Result = <span class="kwrd">new</span> HttpUnauthorizedResult();
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

This is the most basic form of a check. “The identity of the principal is not authenticated, so set the result to unauthorized, an HTTP 401 status code.” Now we’re cooking with peanut oil and our 401 will not let the user access the action until they satisfy whatever arguments you’d like them to fulfill.&nbsp; To that end, you don’t _only_ need to use the principal identity, you can use whatever you like, such as a tie-in to a third-party SSO provider in a mixed identity environment.

Next, we have to deal with the fact that someone who is unauthenticated tried to access a resource we were protecting. The way the Authorize attribute worked was just by letting that 401 we set above flow through the framework. In an MVC application the default mechanism for authentication is Forms, for which there is a default account controller and corresponding views added to our project. Thus, by simply letting the user “fall through” here, they will end up at the login page.&nbsp; For the following code in your challenge method:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">void</span> OnAuthenticationChallenge(AuthenticationChallengeContext filterContext)
{
    <span class="rem">// redirect the user to some form of log in</span>
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

…the user will keep that 401 state through, and will ultimately be bounced back to the site’s login page.

[![image](https://jcblogimages.blob.core.windows.net/img/2013/11/image_thumb.png "image")](https://jcblogimages.blob.core.windows.net/img/2013/11/image1.png)

So, in it’s simplest form as we’ve done above, authentication filters give us a way to remove Authorize attributes (separation of concerns) and mimic the behavior we were previously leveraging to force users to log in. But there’s more, right?

## Practical Uses of Our New Custom Authentication Bestie

We get pretty good support with the Authorize attribute and our ability to create custom filters already, but if we agree that separation of concerns is important, we get a few benefits with the new Authentication filters and the timing with which their methods are fired.

### 

### Isolated Custom Authentication

One of the first possibilities is that you can now setup an action or controller to have a custom authentication mechanism, rather than using whatever the default is for the site. If you have an external provider (perhaps something legacy) that you can proxy credentials to, this might be one way you could approach this.

### Customizing UI For Login

There may be some cases where you want to do something like augment credentials, or perhaps configure part of the application, or even let users log in with a single-use code. OnAuthenticationChallenge lets you configure the result, so you can route the user to whatever view you like in your application.

## 

## Things to Remember

I actually don’t have a lot of caveats to suggest here, but perhaps the most important bit to remember is that if you IAuthenticationFilter.OnAuthenticate fall through without a 401, you’re letting the user into the action. Your OnAuthenticateChallenge will still fire, but remember from the breakpoints set above (when we were just spitting out debug lines) that the action _will_ fire and your code _will _be executed. Logging the user in at this point happens after the invocation…a worst case scenario would be on a POST where an update is happening.

Finally, remember that Authentication wraps Authorization and action execution, and the Authenticate method precedes your first opportunity to interact with the request pipeline when you use ActionFilterAttribute inheritance on its own.

## Next Steps

It’s pretty easy to try this out! Why not give the following a try?

*   File –&gt; New –&gt; Project and give the exercise above a try
*   Think of other uses or scenarios where one could leverage this
*   Break out your custom authentication filters built on the old objects in older versions of the MVC Framework

Happy coding!

_Thanks to good friend [David Paquette](https://twitter.com/Dave_Paquette) who reviewed and suggested updates on this post._