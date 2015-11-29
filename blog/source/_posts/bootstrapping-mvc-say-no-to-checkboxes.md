title: Bootstrapping Mvc - Say No to Checkboxes
tags:
  - Asp.Net MVC
  - Bootstrap
  - MVC
  - Visual Studio 2012
id: 681
categories:
  - Code Dive
  - Projects
date: 2012-07-08 17:47:00
---

We’re diving deeper today and getting to some of the good stuff, in particular, we’re going to start muxing the best of Bootstrap (the jQuery plugins) with some of the framework features Mvc has to offer.&nbsp; Again, I'm working through the CSS and JavaScript library called "Bootstrap" from Twitter, and munging it in with Asp.Net's Mvc Framework using Visual Studio 2012 RC.

The default display templates for boolean property values can be quite different – nullables are rendered as drop downs, non-nullable bools are checkboxes – but we have the ability to override the Framework and change the way these are rendered.&nbsp; This video walks you through, and of course, you can download the code from GitHub [on this project's page.](htt[://github.com/MisterJames/BootstrappingMvc) You can also keep up with this entire series from the intro page, where I’m listing all of the posts related to these posts.

Here’s today’s entry:
 <iframe width="560" height="315" src="http://www.youtube.com/embed/4_Lqq6ZUj2A" frameborder="0" allowfullscreen></iframe> 

The video is in HD, so make sure you full-screen-a-size-it to see the code.&nbsp; Speaking of code...

## The Code That Makes it Tick

What I did for this post was override the default templates in MVC that are used to render boolean properties. If you use DisplayFor and EditorFor explicitly in your views, or if you use the scaffolders that are part of the MVC tooling, then you may be familiar with the controls that are rendered. They are fine, but they just don't look like the rest of our site when we're using Bootstrap.

For our DisplayTemplate, I'm using Bootstrap's label classes on spans. You can't edit the properties when you're in 'display' mode anyways, so let's try to pretty it up a bit.&nbsp; Put this code in a partial view called Boolean.cshtml under Views/Shared/DisplayTemplates:
<pre class="csharpcode">@model <span class="kwrd">bool</span>?

@<span class="kwrd">if</span> (Model.HasValue)
{
    <span class="kwrd">if</span> (Model.Value)
        { &lt;span <span class="kwrd">class</span>=<span class="str">"label label-success"</span>&gt;Yes&lt;/span&gt; }
    <span class="kwrd">else</span>
        { &lt;span <span class="kwrd">class</span>=<span class="str">"label label-important"</span>&gt;No&lt;/span&gt; }
}
<span class="kwrd">else</span>
    { &lt;span <span class="kwrd">class</span>=<span class="str">"label label-inverse"</span>&gt;Not Set&lt;/span&gt; }</pre>

&nbsp;

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

And for the editor, in EditorTemplates, you'll need a Boolean.cshtml partial view as well. Here, we're setting up some vars that will allow us to make use of the new nullable class attributes in MVC4.&nbsp; Then, we render yes/no buttons and _conditionally_ render the "do not set" button based on the field's metadata.
<pre class="csharpcode">    @model <span class="kwrd">bool</span>?

    @{
        <span class="rem">// make use of MVC4 nullable class attribute values</span>
        var yesSelected = Model.HasValue &amp;&amp; Model.Value ? <span class="str">"active"</span> : <span class="kwrd">null</span> ;
        var noSelected = Model.HasValue &amp;&amp; !Model.Value ? <span class="str">"active"</span> : <span class="kwrd">null</span>;
        var noSelection = !Model.HasValue ? <span class="str">"active"</span> : <span class="kwrd">null</span>;   

        <span class="rem">// get the name of the ID - this is to support multiple fields     </span>
        var htmlField = ViewData.TemplateInfo.HtmlFieldPrefix;
    }

    @Html.HiddenFor(model =&gt; model)

    &lt;div <span class="kwrd">class</span>=<span class="str">"btn-group"</span> data-toggle=<span class="str">"buttons-radio"</span>&gt;
        &lt;button type=<span class="str">"button"</span> <span class="kwrd">class</span>=<span class="str">"btn btn-info @yesSelected bool-@htmlField"</span> onclick=<span class="str">"javascript:$('#@htmlField').val(true);"</span> &gt;Yes&lt;/button&gt;
        &lt;button type=<span class="str">"button"</span> <span class="kwrd">class</span>=<span class="str">"btn btn-info @noSelected bool-@htmlField"</span> onclick=<span class="str">"javascript:$('#@htmlField').val(false);"</span> &gt;No&lt;/button&gt;

        @<span class="kwrd">if</span> (ViewData.ModelMetadata.IsNullableValueType)
            { &lt;button type=<span class="str">"button"</span> <span class="kwrd">class</span>=<span class="str">"btn btn-info @noSelection bool-@htmlField"</span> onclick=<span class="str">"javascript:$('#@htmlField').val('');"</span> &gt;Do Not Set&lt;/button&gt; }

    &lt;/div&gt;</pre>

## Wrapping Up

Again, the project is on [GitHub](http://github.com/misterjames/bootstrappingmvc) for you to follow along or experiment, but it's very easy to File –&gt; New Project and follow along with the video.&nbsp; We're going to keep working through the jQuery plugins and find uses – contrived if we have to! – for each one. 

If you don't already have a copy of Visual Studio 2012, [get it here](http://www.microsoft.com/visualstudio/11/en-us/downloads).&nbsp; Happy coding!

Cheers!