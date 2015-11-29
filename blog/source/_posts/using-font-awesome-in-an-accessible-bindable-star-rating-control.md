title: 'Using Font Awesome in an Accessible, Bindable Star Rating Control'
tags:
  - Bootstrap
  - MVC5
id: 5011
categories:
  - Code Dive
date: 2014-09-08 02:00:49
---

<font size="1">
</font>[![banner](http://jameschambers.com/wp-content/uploads/2014/09/banner.png "banner")](https://leanpub.com/bootstrappingmvc "Bootstrapping MVC - THE eBook for developers using Bootstrap on MVC")

Creating a rating control that works across browsers, looks good and can be taken advantage of by the MVC Framework requires a little CSS know-how, a couple of freely-available frameworks and some convention-following. There’s a bit of work to get it set up, but it’s reusable, accessible and gosh darn it, it looks good, too.

Don’t overlook the accessible bits! It’s important to remember that most organizations and agencies have policies in place ensuring that folks with disabilities can use alternate methods to view your site. While I am by no means an expert in this area I do have the help of a friend who will try things out for me from time to time in a screen reader and I try to keep these concerns at the front of my mind when doing something “off course”.

## Our Starting Point

I’m building off of [the work in the last post](http://jameschambers.com/2014/08/adding-some-font-awesome-to-mvc-and-bootstrap/ "Adding Font Awesome to MVC 5 Projects"), where I added Font Awesome to our Bootstrap-enabled MVC 5 web site. Let’s drop a model class for a movie in our project in the Models folder that looks like this:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> Movie
{
    <span class="kwrd">public</span> <span class="kwrd">int</span> MovieId { get; set; }
    <span class="kwrd">public</span> <span class="kwrd">string</span> Title { get; set; }
    [UIHint(<span class="str">"StarRating"</span>)]
    <span class="kwrd">public</span> <span class="kwrd">int</span> Rating { get; set; }
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

Next, let’s update the HomeController to have the following actions:
<pre class="csharpcode"><span class="kwrd">public</span> ActionResult Index()
{
    var movie = <span class="kwrd">new</span> Movie
    {
        Title = <span class="str">"The Last Starfighter"</span>, 
        Rating = 4
    };
    <span class="kwrd">return</span> View(movie);
}

[HttpPost]
<span class="kwrd">public</span> ActionResult Index(Movie movie)
{
    <span class="kwrd">return</span> View(<span class="str">"Index"</span>, movie);
}</pre>

Nothing too fancy here, just a GET and POST action for Index requests. The GET populates (simulates) someone navigating to an item that they can rate. The POST allows you to set a breakpoint so that you can inspect the movie result when you submit.

Finally, scaffold a view for Index. **First**, delete the Index.cshtml from the Views/Home folder in your project. **Next**, right-click on one of the the Index methods above and select Add View, then choose the Edit template (don’t create a partial view here) and let it build the page for you.&nbsp; **Finally**, run your project to see what you get; it should look like this:

![image](http://jameschambers.com/wp-content/uploads/2014/09/image.png "image")

That’s okay, but, here’s what we want to see:

[![image](http://jameschambers.com/wp-content/uploads/2014/09/image_thumb.png "image")](http://jameschambers.com/wp-content/uploads/2014/09/image1.png)

## Getting Stylish

So, believe it or not, those are just radio buttons! There’s a couple of things we do to make it come together, including the use of a container DIV, but it’s mostly straightforward. Let’s look at the basic HTML structure that makes this tick.
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="rating"</span><span class="kwrd">&gt;</span>
  <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">id</span><span class="kwrd">="star5-Rating"</span> <span class="attr">name</span><span class="kwrd">="Rating"</span> <span class="attr">value</span><span class="kwrd">="5"</span><span class="kwrd">&gt;</span>
  <span class="kwrd">&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="star5-Rating"</span> <span class="attr">title</span><span class="kwrd">="Best"</span><span class="kwrd">&gt;</span>5 stars<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>

  <span class="rem">&lt;!-- ...more like these... --&gt;</span>

  <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">id</span><span class="kwrd">="star1-Rating"</span> <span class="attr">name</span><span class="kwrd">="Rating"</span> <span class="attr">value</span><span class="kwrd">="1"</span><span class="kwrd">&gt;</span>
  <span class="kwrd">&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="star1-Rating"</span> <span class="attr">title</span><span class="kwrd">="Pretty Bad"</span><span class="kwrd">&gt;</span>1 star<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
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

The container holds a collection of inputs and labels. These elements are attributed as you’d expect, but are inserted in the document in _reverse order_ so that “5 stars” is at the top and “1 star” is at the bottom. This has to do with CSS bits, which we’ll come to in a minute.

## Oh…here we are, the CSS bits! 

Right, so we start by setting up the container and getting our input off-screen. 
<pre class="csharpcode">.rating {
    float:left;
}

.rating:not(:checked) <span class="kwrd">&gt;</span> input {
    position:absolute;
    top:-9999px;
    clip:rect(0,0,0,0);
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

This works okay because if a user taps, clicks or selects a label with a screen reader, the input is still appropriately checked. Next, we wire up the label.
<pre class="csharpcode">.rating:not(:checked) <span class="kwrd">&gt;</span> label {
    float:right;
    width:1em;
    padding:0 .1em;
    overflow:hidden;
    white-space:nowrap;
    cursor:pointer;
    font-size:3em;
    line-height:1.2;
    color:#e0e0e0;
}

.rating:not(:checked) <span class="kwrd">&gt;</span> label:before {
    font-family: FontAwesome;
    content: "\f005";
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

These two styles push the label off-screen and setup the fixings for our star, including the insert of the character from Font Awesome. 

We float these to the right so that we correct our order; remember that we inserted the stars in reverse order in our HTML, here we use CSS to correct that. This workaround is needed for the foreseeable future as we do not have CSS selectors to help us resolve things “before” our element, only after. By putting rating 1 _after_ rating 3, we can select 3, 2 and 1 with various combinations of selectors; however, there is no mechanism allowing us to select the other way, i.e., “before”.&nbsp; (The before psuedo-selector allows us to insert content, but not apply style transformations.)

Finally, we handle the color states with various combinations to handle selected stars, stars that were previously selected and stars that are hovered (but not yet selected).
<pre class="csharpcode">.rating <span class="kwrd">&gt;</span> input:checked ~ label {
    color: #fa0;
}

.rating:not(:checked) <span class="kwrd">&gt;</span> label:hover,
.rating:not(:checked) <span class="kwrd">&gt;</span> label:hover ~ label {
    color: #fe0;
}

.rating <span class="kwrd">&gt;</span> input:checked + label:hover,
.rating <span class="kwrd">&gt;</span> input:checked + label:hover ~ label,
.rating <span class="kwrd">&gt;</span> input:checked ~ label:hover,
.rating <span class="kwrd">&gt;</span> input:checked ~ label:hover ~ label,
.rating <span class="kwrd">&gt;</span> label:hover ~ input:checked ~ label {
    color: #ea0;
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

It may seem like there’s a lot of action going on in that code, but it’s required. The &gt;, +, ~ and : selectors in there allow us to be very specific about which elements _in which state_ require coloring.

Add all of those CSS bits to your Site.css file (in the Content folder) and your styles will be in place. All that’s left to do now is to take that rough-in of HTML we did before and turn it into something the MVC Framework can use.

## Bringing Some MVC to the Equation

You’ll have noticed the attribute UIHint decorating our Rating property on the Movie model class that we created in the first step. That was no accident! Now, let’s get that working for us by creating a reusable partial view in our project. Add a file called StarRating.cshtml to the Views\Shared\EditorTemplates folder. By convention, this is the folder that the MVC Framework checks for custom views used to render properties or models (you can see another [sample here from my Bootstrap series](http://jameschambers.com/2014/06/day-7-semi-automatic-bootstrap-display-templates/ "make checkboxes better with Bootstrap and the MVC Framework")). 

The StarRating.cshtml should look like this:
<pre class="csharpcode">@model int

@{
    var chk = "checked";
    var checked1 = Model == 1 ? chk : null;
    var checked2 = Model == 2 ? chk : null;
    var checked3 = Model == 3 ? chk : null;
    var checked4 = Model == 4 ? chk : null;
    var checked5 = Model == 5 ? chk : null;
    var htmlField = ViewData.TemplateInfo.HtmlFieldPrefix;
}

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="rating"</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">checked</span><span class="kwrd">="@checked5"</span> <span class="attr">id</span><span class="kwrd">="star5-@htmlField"</span> <span class="attr">name</span><span class="kwrd">="@htmlField"</span> <span class="attr">value</span><span class="kwrd">="5"</span> <span class="kwrd">/&gt;&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="star5-@htmlField"</span> <span class="attr">title</span><span class="kwrd">="Best"</span><span class="kwrd">&gt;</span>5 stars<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">checked</span><span class="kwrd">="@checked4"</span> <span class="attr">id</span><span class="kwrd">="star4-@htmlField"</span> <span class="attr">name</span><span class="kwrd">="@htmlField"</span> <span class="attr">value</span><span class="kwrd">="4"</span> <span class="kwrd">/&gt;&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="star4-@htmlField"</span> <span class="attr">title</span><span class="kwrd">="Good"</span><span class="kwrd">&gt;</span>4 stars<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">checked</span><span class="kwrd">="@checked3"</span> <span class="attr">id</span><span class="kwrd">="star3-@htmlField"</span> <span class="attr">name</span><span class="kwrd">="@htmlField"</span> <span class="attr">value</span><span class="kwrd">="3"</span> <span class="kwrd">/&gt;&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="star3-@htmlField"</span> <span class="attr">title</span><span class="kwrd">="Average"</span><span class="kwrd">&gt;</span>3 stars<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">checked</span><span class="kwrd">="@checked2"</span> <span class="attr">id</span><span class="kwrd">="star2-@htmlField"</span> <span class="attr">name</span><span class="kwrd">="@htmlField"</span> <span class="attr">value</span><span class="kwrd">="2"</span> <span class="kwrd">/&gt;&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="star2-@htmlField"</span> <span class="attr">title</span><span class="kwrd">="Not Great"</span><span class="kwrd">&gt;</span>2 stars<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="radio"</span> <span class="attr">checked</span><span class="kwrd">="@checked1"</span> <span class="attr">id</span><span class="kwrd">="star1-@htmlField"</span> <span class="attr">name</span><span class="kwrd">="@htmlField"</span> <span class="attr">value</span><span class="kwrd">="1"</span> <span class="kwrd">/&gt;&lt;</span><span class="html">label</span> <span class="attr">for</span><span class="kwrd">="star1-@htmlField"</span> <span class="attr">title</span><span class="kwrd">="Pretty Bad"</span><span class="kwrd">&gt;</span>1 star<span class="kwrd">&lt;/</span><span class="html">label</span><span class="kwrd">&gt;</span>
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

Here’s a breakdown of the interesting bits that make this work:

*   The model is of type int, allowing it to be rendered for properties of the same type.
*   There is the use of nullable attributes in play, set up with the check against the model so that we can correctly render _which_ star is to be selected. This is important when our model already has a value.
*   The radio buttons are ordered in reverse and take their name from the ViewData.TemplateInfo.HtmlFieldPrefix value.*   We use an ID on each star, and a label that is related back to that input. This allows selection via the label (you should always use this!)

Viola! With the CSS from above in place, the custom editor template saved and the UIHint on your Movie model class, your page will now be rendered with a star rating control for your movie.

Set a breakpoint in your POST action of your controller. If you submit your form while debugging, you will see that the value you have selected is indeed bound to the property of the Movie class. Huzzah!

## Credits

I have leaned on many star rating controls over the last decade, but have come back to variants of this one ([based on this work](http://lea.verou.me/2011/08/accessible-star-rating-widget-with-pure-css/) from [@Lea Verou](https://twitter.com/leaverou)) for the last two or three years now. I would like to stress that I haven’t yet found a _perfect_ control (this one, for example works well with up/down arrows, but not left/right) and I am sure this version won’t work for all people in all scenarios. Also, the idea for this example came from the [Font Awesome site](http://fontawesome.io/examples/ "Font Awesome Examples"), but the astute reader would soon realize why the basic example on their site would not be a good candidate for MVC (primarily because of the lack of structure to accommodate multiple controls on the same page).

## Next Steps

If you’ve built the sample along with the article, you’ll likely have had at least a few ideas on how you can create reusable controls for your project. Be sure to check out my [Bootstrap series](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/) or purchase [a copy of my book](https://leanpub.com/bootstrappingmvc) for more examples like this, and happy coding!