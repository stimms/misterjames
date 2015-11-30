title: 'Day 8: Semi-Automatic Bootstrap – Editor Templates'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3311
categories:
  - Code Dive
date: 2014-06-08 10:47:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Now that we have a nice way to consistently display our data, what about editing it? A label is fine to indicate what the saved value is, but it doesn’t really solve the issue of input.

## Introducing Editor Templates

Much in the same way that a shared view can act as the de facto template for rendering data ([as we saw yesterday](http://jameschambers.com/2014/06/day-7-semi-automatic-bootstrap-display-templates/)), you can override the default editor output by the framework. Create a new view in Views\Shared\EditorTemplates called Boolean.cshtml and put the following code in it:
<pre class="csharpcode">@model <span class="kwrd">bool</span>?

@{
    <span class="rem">// make use of nullable class attribute values</span>
    var yesSelected = Model.HasValue &amp;&amp; Model.Value ? <span class="str">"active"</span> : <span class="kwrd">null</span>;
    var noSelected = (Model.HasValue &amp;&amp; !Model.Value)
        || (!Model.HasValue &amp;&amp; ViewData.ModelMetadata.IsNullableValueType) 
        ? <span class="str">"active"</span> 
        : <span class="kwrd">null</span>;
    var noSelection = !Model.HasValue ? <span class="str">"active"</span> : <span class="kwrd">null</span>;

    <span class="rem">// get the name of the ID - this is to support multiple fields</span>
    var htmlField = ViewData.TemplateInfo.HtmlFieldPrefix;
}

@Html.HiddenFor(model =&gt; model)

&lt;div <span class="kwrd">class</span>=<span class="str">"btn-group"</span> data-toggle=<span class="str">"buttons"</span>&gt;
    &lt;label <span class="kwrd">class</span>=<span class="str">"btn btn-info @yesSelected"</span>&gt;
        &lt;input type=<span class="str">"radio"</span> <span class="kwrd">class</span>=<span class="str">"bool-@htmlField"</span> onchange=<span class="str">"javascript:$('#@htmlField').val(true);"</span> /&gt; Yes
    &lt;/label&gt;
    &lt;label <span class="kwrd">class</span>=<span class="str">"btn btn-info @noSelected"</span>&gt;
        &lt;input type=<span class="str">"radio"</span> <span class="kwrd">class</span>=<span class="str">"bool-@htmlField"</span> onchange=<span class="str">"javascript:$('#@htmlField').val(false);"</span> /&gt; No
    &lt;/label&gt;

    @<span class="kwrd">if</span> (ViewData.ModelMetadata.IsNullableValueType)
    {
        &lt;label <span class="kwrd">class</span>=<span class="str">"btn btn-info @noSelection"</span>&gt;
            &lt;input type=<span class="str">"radio"</span> <span class="kwrd">class</span>=<span class="str">"bool-@htmlField"</span> onclick=<span class="str">"javascript:$('#@htmlField').val('');"</span> /&gt;Do Not Set
        &lt;/label&gt;

    }

&lt;/div&gt;</pre>
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

There are two important pieces to note in the above code, namely the inspection of TemplateInfo and ModelMetadata in the ViewData instance presented to our view, and the hidden backing field that is kept in sync via JavaScript. ViewData is a ViewDataDictionary that contains, as those properties suggest, metadata about the type of model being used, information about the template, and other view-specific data.

To see this new template in action we’ll have to get a Create view set up and an action on our controller. Head back to the SimpleController class and add the following code:
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult Create()
{
    var person = <span class="kwrd">new</span> Person();
    <span class="kwrd">return</span> View(person);
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

Now, right-click on the name of the method, and select “Add View…”, then set it up to use the Create template for the Person class.

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image16.png "image")

Visual Studio will throw you into the editor for your new view, and you can press CTRL+F5 to see your new default control for Boolean values, or navigate to <a>http://localhost:_port_/Simple/Create</a> to see the page. 

The scaffolded view contains simple calls to HTML helpers and doesn’t know anything about the instance of the Person that will be created or the fact that you’ve created a new template to render Boolean values. You’ll only see the following:
<pre class="csharpcode">@Html.EditorFor(model =&gt; model.LikesMusic)</pre>
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

As well, the templates are rendered on the fly by the view engine (just like all views) so you don’t need to recompile as you make updates. Feel free to experiment with the template code and just refresh in your browser after you save.

Note that our Person class doesn’t have a nullable Boolean value, but it if did, it would render like so because of our evaluation of the ModelMetadata in the template above:

![image](https://jcblogimages.blob.core.windows.net/img/2014/06/image17.png "image")

## Controlling the Use of Custom Templates

Now these new controls – button groups for input and labels for display – work great, but you may not wish to use them for all Boolean properties. In both the DisplayTemplates and EditorTemplates folders, rename the Boolean template to BooleanButtonLabel.cshtml. Then, return to your Person class and decorate the LikesMusic property as follows:
<pre class="csharpcode">[UIHint(<span class="str">"BooleanButtonLabel"</span>)]
<span class="kwrd">public</span> <span class="kwrd">bool</span> LikesMusic { get; set; }</pre>
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

There may be scenarios where you wish to use several alternate templates to display or edit data. This attribute gives the MVC Framework directions to use a template of our choosing, rather than having to use the same template for all properties.

## Next Steps

You can now render simple properties with whichever template you wish, but what about more complex types? Tomorrow we’ll look at using a custom template at a class level.