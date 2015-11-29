title: "A Web Developer's Look at the MVC4 Request Pipeline"
tags:
  - ActionFilters
  - Asp.Net MVC
  - CodeProject
  - Routing
id: 461
categories:
  - Code Dive
  - Develop Meta
date: 2012-10-24 17:25:00
---

This article is intended to give you a solid understanding of how client requests move through the Asp.Net Mvc 4 Pipeline.&nbsp; As developers, we need to know what options we have to override default behaviour, modify or augment requests as they arrive (and as they're executed) and extend the pipeline as our requirements dictate.

A friend recently asked why his constructor was being called multiple times on the "same" request.&nbsp; I thought the best way illustrate why this happens would be to explain the entire request stack.&nbsp; Or say, "Just because!", but I also enjoy writing articles like this, so we'll go with the entire stack version.

If you're looking for the "Coles notes" version, scroll down about halfway to see the summary. ![Smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/f726bca5cc21_12811/wlEmoticon-smile_2.png)

# Once Upon a Browser

I'll skip a few steps (like keyboard interrupts on the client device) and skip to the part where the HTTP request enters the interesting bits of the MVC Framework.

On the first request, application startup is executed from Global.asax, which configures the following:

*   Routes for WebAPI  <li>Global filters  <li>General routing  <li>Bundling and minification  <li>Authorization providers 

Bundling and minification relate to client-required resources such as scripts and stylesheets and don't affect our core request.&nbsp; Authorization providers – for FaceBook, Twitter, Microsoft Account and Google Account – are now supported out-of-box, but don't relate directly to our method execution either.

Assuming the app is already running, we can get back to the incoming request.

The registered route handler (an <span style="font-family: consolas" face="Consolas">MvcRouteHandler</span> by default) won't find a physical file for our controller action, so instead it associates a matching route in the <span style="font-family: consolas" face="Consolas">RouteCollection</span> (which we configured above) and gets the <span style="font-family: consolas" face="Consolas">IHttpHandler</span> that will be used for the request.&nbsp; In our case, it's an <span style="font-family: consolas" face="Consolas">MvcHandler</span>, and it's instantiated with the current instance of the <span style="font-family: consolas" face="Consolas">RequestContext</span>.

There are some other pipeline events that are firing, true, but now, we're moving into the interesting bits as a web dev (versus someone developing HTTP handlers).&nbsp; **The MvcHandler is now processing the request**.&nbsp; If you haven't modified the default behaviour, a <span style="font-family: consolas" face="Consolas">DefaultControllerFactory</span> is spun up and creates the controller.

As we ultimately inherit all our controllers from <span style="font-family: consolas" face="Consolas">System.Web.Mvc.Controller</span> we have a few interesting events around the time our action (a public, non-overloaded, non-static class method) is being executed that we can participate in.

*   OnActionExecuting  <li>OnActionExecuted  <li>OnAuthorization  <li>OnException  <li>OnResultExecuting  <li>OnResultExecuted 

However, we also have the ability to create our own filters, which is arguably a better way to interact with the request.

So, filter execution then kicks in, in this order:

*   Authorization  <li>Action  <li>Result  <li>Exception 

Note that the final order of execution is tied to the following orders of precedence:

*   Global filters  <li>Controller filters  <li>Action filters  <li>The order in which those were applied (within their type) 

Our method execution and view rendering are wrapped up in there as well.&nbsp; You can see this by inheriting from <span style="font-family: consolas" face="Consolas">ActionFilterAttribute</span> and overriding the Action and Result events.&nbsp; Put this attribute on your action and you'd see something like this if you were Debug.WriteLining:

*   OnActionExecuting  <li>Action (method) executing  <li>OnActionExecuted  <li>OnResultExecuting  <li>View executing  <li>OnResultExecuted 

One of the great features of the MVC Framework is model binding, which is part of this request pipeline but really deserves it's own article.&nbsp; Model binding is the process in which incoming HTTP data - such as form data or query string parameters - are converted to .Net types.&nbsp; This means that we don't need to cast a form field to an int, but more interestingly, that the form collection (or querystring parameters) as a whole can be converted to complex .Net types like a Person object. (I have some samples on this [here](http://theycallmemrjames.blogspot.ca/2010/05/aspnet-mvc-and-jquery-part-4-advanced.html).)

There's one last point worth mentioning here. When the view is being executed it is done so by the registered view engine(s).&nbsp; You can replace this with anything available in the community or you can create your own.&nbsp; There are a couple of exceptions to this part of execution:

*   You are not returning a result that derives from ViewResult, in which case your method is executed and the result is returned to the client. You can set your response types (like 403, or 200) explicitly.  <li>Your action is an EmptyResult, or it returns void which is translated into an empty result. Again, you can set your response types as you wish.  <li>Your action generates an exception, in which case the last piece of your code being executed may just very well be your custom exception handler. 

# What About the Coles Notes?

A shorthand version of the following might be as follows:

*   Incoming request  <li>App initialization (on the first request to the application)  <li>Routing magic/kung fu  <li>Controller creation  <li>Filter execution  <li>Action invocation  <li>Filter execution  <li>View Rendering  <li>Final filter execution 

And BAM! Your user sees something in the browser.

# Riddle Me This

Now, a quick quiz.&nbsp; Given the following view:
<pre class="csharpcode">    <span class="kwrd">&lt;</span><span class="html">h2</span><span class="kwrd">&gt;</span>Execution Count Testing<span class="kwrd">&lt;/</span><span class="html">h2</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
        The execution count from the view is: 
        @Model. (@MvcApplication3.MvcApplication.ConstructorCallCount 
        constructor calls.)
    <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
        Rendered from the partial view via Html.RenderAction, the 
        execution count is: @{Html.RenderAction("ExecutionCountAsPartial");}.
    <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>
        Rendered via ajax request from the partial view, the execution 
        count is: <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">id</span><span class="kwrd">="count-result"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>

    @section scripts
    {
    <span class="kwrd">&lt;</span><span class="html">script</span> <span class="attr">type</span><span class="kwrd">="text/javascript"</span><span class="kwrd">&gt;</span>
        $(<span class="kwrd">function</span> () {
            <span class="kwrd">var</span> requestUrl = <span class="str">'@Url.Action("ExecutionCountAsPartialViaAjax")'</span>;
            $.ajax(requestUrl)
            .success(<span class="kwrd">function</span> (data) {
                $(<span class="str">"#count-result"</span>).html(data);
            });
        });
    <span class="kwrd">&lt;/</span><span class="html">script</span><span class="kwrd">&gt;</span>
    }</pre>

...and the following controller code:
<pre class="csharpcode">    <span class="kwrd">private</span> <span class="kwrd">int</span> _myCounter = 0;

    <span class="kwrd">public</span> HomeController()
    {
        MvcApplication.ConstructorCallCount++;
    }

    <span class="kwrd">public</span> ActionResult ExecutionCount()
    {
        <span class="kwrd">return</span> View(++_myCounter);
    }

    <span class="kwrd">public</span> PartialViewResult ExecutionCountAsPartial()
    {
        <span class="kwrd">return</span> PartialView(++_myCounter);
    }

    [OutputCache(NoStore=<span class="kwrd">true</span>, Duration=1)]
    <span class="kwrd">public</span> PartialViewResult ExecutionCountAsPartialViaAjax()
    {
        <span class="kwrd">return</span> PartialView(<span class="str">"ExecutionCountAsPartial"</span>, ++_myCounter);
    }</pre>

...would you have thought you would have seen this?

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/f726bca5cc21_12811/image_6.png "image")

Back to our original question, we have to differentiate between what a client request is and what you might refer to as an execution request.&nbsp; In this case, the view for ExecutionCount is invoking three "execution" requests across two "client" requests.&nbsp; The first two action invocations are made through the first client request, and when that is returned to the browser, the client is making a second request via Ajax which results in the third method execution.

So, two requests from the client and three actions executed, and the constructor is called three times.&nbsp; We're trackin'.&nbsp; So, why all the controller constructor calls?

Well, let's turn this around a second.&nbsp; What if different actions are attributed with different filters?&nbsp; Authorization requirements?&nbsp; What about out-of-order execution (as in, single page applications making requests on whatever timing plays out)? The result in any of those would be a design where we're no longer able to DoOneThing and we'd have to write composite filters that were able to handle the permutations of the above and other scenarios.&nbsp; The MVC pipeline is designed such that each request passes through the above steps, regardless of where the request comes from.&nbsp; This ensures consistency in execution for all requests.

If you are looking for different behaviour you can work around this if you simply separate your concerns: what I'm saying here is that a controller shouldn't be used to do heavy lifting.&nbsp; My answer to my friend was simply that his constructor was likely doing too much work and, in his case, user state shouldn't be managed in the constructor.

# More Reading

Now, all that is likely enough for most of us, but if you want to dive deeper you can read up on the following topics in more details:

*   [MvcHandler](http://msdn.microsoft.com/en-us/library/system.web.mvc.mvchandler(v=vs.108).aspx)<li>[Filtering in Asp.Net MVC](http://msdn.microsoft.com/en-us/library/gg416513(v=vs.98).aspx)<li>[Mvc Routing](http://msdn.microsoft.com/en-us/library/cc668201(v=vs.100).aspx)

As well, code from this article is available in the following repository on GitHub:

[https://github.com/MisterJames/AspNetMvc-Execution-Pipeline](https://github.com/MisterJames/AspNetMvc-Execution-Pipeline)