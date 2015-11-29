title: 'Day 12: Implement Search Using Inline Forms and AJAX'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3541
categories:
  - Code Dive
date: 2014-06-12 22:18:27
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

It had to happen; at some point we were going to need to let our users enter some data! Well, that time has come, so let’s start by adding a handy-dandy search form to our Person page.

Forms will need a few cues on how to properly render themselves and take part in Booststrap’s style party of awesome. Let’s get a search form going and start filtering our results.

## Adding the Search Bar

There are actually a few styles of forms that you can get going. A standard styling gives you label-over-control type layout, horizontal forms give you label-beside-control layout and inline styling gives you controls without labels side-by-each continuously in the row. That’s the one we’ll go with to generate our simple search form:

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb6.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image20.png)

Create a partial view under the Views\Person folder. You can make it an empty one, and call it _PersonSearchForm.cshtml.&nbsp; Paste in the following code:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">hr</span><span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="container"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="pull-right"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">form</span> <span class="attr">class</span><span class="kwrd">="form-inline"</span> <span class="attr">role</span><span class="kwrd">="form"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="form-group"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">label</span> <span class="attr">class</span><span class="kwrd">="sr-only"</span> <span class="attr">for</span><span class="kwrd">="search-text"</span><span class="kwrd">&gt;</span>Email address<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="text"</span> <span class="attr">class</span><span class="kwrd">="form-control"</span> <span class="attr">id</span><span class="kwrd">="search-text"</span> <span class="attr">placeholder</span><span class="kwrd">="Enter Search Text"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">type</span><span class="kwrd">="button"</span> <span class="attr">class</span><span class="kwrd">="btn btn-success"</span> <span class="attr">id</span><span class="kwrd">="search-btn"</span><span class="kwrd">&gt;</span>Search<span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">form</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
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

We have a few things going on here:

*   The HR tag is just a style thing and not required. It makes the search form look more “balanced” vertically on the form.<li>There is a DIV that acts as a container, keeping our content separate from the rest of the page. It is a wrapper for a “pull-right” styled DIV that moves our search bar over to the right hand side of the page.<li>The form is given a class of “form-inline”. This is the first important part of making our controls and labels show up correctly.<li>Inside that FORM element we have a DIV with a class of “form-group”. This lets Bootstrap know (or rather, the browser through Bootstrap’s CSS) that these controls are related in some way. Specifically, we have a label for an input.<li>Because this is an “inline” form, we’re using the “sr-only” class on the label to eat the display and keep the visuals tidy. “SR” stands for “Screen Reader”; this is an accessibility tag.

When the form data comes calling, we’re going to need someone to answer the phone on the controller side.

## The Controller’s Search Method

In your PersonController class, add the following public method:
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult SearchPeople(<span class="kwrd">string</span> searchText)
{
    var term = searchText.ToLower();
    var result = _people
        .Where(p =&gt;
            p.FirstName.ToLower().Contains(term) ||
            p.LastName.ToLower().Contains(term) 
        );

    <span class="kwrd">return</span> PartialView(<span class="str">"_SearchPeople"</span>, result);
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

This accepts a string parameter and finds any matches where FirstName or LastName match what the user entered, then it returns via a call to PartialView to generate the result. We’re using a partial because we don’t want to have to reload the entire page each time the user clicks the search button.

> **A quick note**: the astute reader will note that that this simplified method of search won’t pass the [Turkey Test](http://www.moserware.com/2008/02/does-your-code-pass-turkey-test.html). If you work with different cultures, this is a great side-read, and you’ll need to approach the problem in a different way.

Later in the series we’ll cover best practices around accessing and filtering data, but this approach will suffice for now, using the local static collection of data that we built [yesterday](http://jameschambers.com/2014/06/day-11-realistic-test-data-for-our-view/).

When we call PartialView the MVC Framework doesn’t attempt to resolve a layout, so we just get the meat that lives in the cshtml file itself and as processed by the view engine. Rendered via the controller, we have to pass in our data as a parameter. If you were rendering a partial via a View (with an HtmlHelper) the partial could ‘inherit’ the parent page’s model and use that to render your content. The partial we need should be located in Views\Person\ and called _SearchPeople.cshtml and the code looks like the following:
<pre class="csharpcode">@model IEnumerable<span class="kwrd">&lt;</span><span class="html">SimpleSite.Models.Person</span><span class="kwrd">&gt;</span>

@Html.DisplayForModel(Model)

@if (!Model.Any())
{
    <span class="kwrd">&lt;</span><span class="html">h3</span><span class="kwrd">&gt;</span>Sorry!<span class="kwrd">&lt;/</span><span class="html">h3</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">p</span><span class="kwrd">&gt;</span>Looks like there's no results for that person.<span class="kwrd">&lt;/</span><span class="html">p</span><span class="kwrd">&gt;</span>
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

Partial in place, controller set up to search…off to the core view.

## Updating the View

Back in Views\Person\Index.cshtml there isn’t a lot we have to do to get our form to display. Update the code so it reads as follows:
<pre class="csharpcode">@model IEnumerable<span class="kwrd">&lt;</span><span class="html">SimpleSite.Models.Person</span><span class="kwrd">&gt;</span>

@{ Html.RenderPartial("_PersonSearchForm"); }

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">id</span><span class="kwrd">="people-data"</span><span class="kwrd">&gt;</span>
    @Html.DisplayForModel(Model)
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span></pre>
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

I have updated the view from yesterday by wrapping the data with a DIV that acts as a container. We’ll use that later when we AJAX up the page. Press CTRL+F5 to see the updated view in action.

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb8.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image21.png)

This handles the display aspect, but we need some script in place to handle the button click and make the AJAX call, finally updating the page with the search results. Add a script section to the bottom of the page as follows:
<pre class="csharpcode">@section scripts
{
    <span class="kwrd">&lt;</span><span class="html">script</span> <span class="attr">type</span><span class="kwrd">="text/javascript"</span><span class="kwrd">&gt;</span>
        $(<span class="kwrd">function</span> () {
            <span class="rem">// it's lonely here...</span>
        });
    <span class="kwrd">&lt;/</span><span class="html">script</span><span class="kwrd">&gt;</span>
}
</pre>
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

> **Today’s Bonus Content**: Sections are defined in the layout page that is used by your view. You can find them in Views\Shared\_Layout.cshtml. These sections can be required or optional per your needs. The default template defines only the scripts section, but you may often wish to include something in the page header or footer. Oooo! Sounds like another blog post!

Now the script doesn’t do anything quite yet, except give us a place to land. What you see above is called a “self-executing anonymous method”, which is a good term to know if you want to sound smart around your boss. Basically, jQuery will make sure any code in this block is executed in a cross-browser friendly way _after_ the page is finished loading. 

Replace that lonely comment with the following code:
<pre class="csharpcode">$(<span class="str">"#search-btn"</span>).click(<span class="kwrd">function</span> () {
    <span class="kwrd">var</span> searchTerm = $(<span class="str">"#search-text"</span>).val();
    $.get(<span class="str">"SearchPeople"</span>, { searchText: searchTerm })
        .success(<span class="kwrd">function</span>(data) {
        $(<span class="str">"#people-data"</span>).html(data);
    });
});</pre>
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

If you’re not familiar with JavaScript or the patterns that jQuery uses, here’s little breakdown of what is happening:

*   A click event handler is setup for the element in the DOM that has an ID of “search-btn”.<li>The event handler is an anonymous method that invokes the jQuery get() method, passing in the action that we’re targeting and the data that we’re trying to pass in.<li>The data we’re passing in is read from the form and assigned to the searchText key<li>When you pass in parameters from calls like this, you need to make sure spelling and case are identical to avoid rapid hair loss. And null values.<li>The get() method follows the “promise” pattern, and you get to register a callback when the search is completed. Here, we use the success callback.<li>Our anonymous callback is invoked when the AJAX completes successfully and it updates the data container (“people-data”) with the HTML that is returned from our controller.

Try typing in some search terms from your Person\Index page.

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb9.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image22.png)

Bazinga!

## Next Steps

Our search form looks fine but wouldn’t be ideal for data entry. Tomorrow we’ll look at the other two variants of forms and wire up a view (and our controller) to allow users to create new people.