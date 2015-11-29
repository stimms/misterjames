title: 'Day 13: Standard Styling and Horizontal Forms'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3651
categories:
  - Code Dive
date: 2014-06-13 22:11:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

With yesterday having a quick look at the inline styling for a search form, it’s only appropriate to have a look at the other types of styling as well. Users are going to want to enter data!

We’re going to make a quick pit stop first to update our Person class so that we can take advantage of a few of the validation features of both MVC and the Bootstrap library.

For you 15-minutes-or-less folks out there, sorry…this is one of the longer posts, but we’re covering a lot of useful ground!

## Updating our Person Model (Class) in Person.cs

We’re going to need to do two things here, add a constructor (so we don’t run into problems with our collection) and make all fields except PersonId and Skills required. The Person class should look like this when you’re done:
<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> Person
{
    <span class="kwrd">public</span> Person()
    {
        Skills = <span class="kwrd">new</span> HashSet&lt;<span class="kwrd">string</span>&gt;();
        BirthDate = DateTime.Now.AddYears(-20);
    }

    <span class="kwrd">public</span> <span class="kwrd">int</span> PersonId { get; set; }

    [Required]
    <span class="kwrd">public</span> <span class="kwrd">string</span> FirstName { get; set; }

    [Required]
    <span class="kwrd">public</span> <span class="kwrd">string</span> LastName { get; set; }

    [Required]
    <span class="kwrd">public</span> DateTime BirthDate { get; set; }

    [Required]
    [UIHint(<span class="str">"BooleanButtonLabel"</span>)]
    <span class="kwrd">public</span> <span class="kwrd">bool</span> LikesMusic { get; set; }

    [Required]
    [EmailAddress]
    <span class="kwrd">public</span> <span class="kwrd">string</span> EmailAddress { get; set; }

    <span class="kwrd">public</span> ICollection&lt;<span class="kwrd">string</span>&gt; Skills { get; set; }
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

Note that on the EmailAddress, we also added an attribute called, well, EmailAddress. This is a pre-defined validation attribute that the Framework uses to give us useful information in our controller, as well as to leverage client-side scripts for validation (to save round-tripping). Best of both worlds!

## Allowing Create on Our Controller

Now we can pop into our controller and set things up for the create action. We’ll need two methods – one for the GET and one for the POST. The GET method creates a default Person object and passes it to the view.
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

The POST method is decorated with an attribute and accepts an instance of Person back as a parameter. This is one of the really great features of the MVC Framework: while you still have access to the form collection and full request object, you don’t have to deal with the cruft unless you want or need to; the Framework performs “model binding” for you and fills in the properties of the parameter objects you are expecting based on the names of the fields passed in.
<pre class="csharpcode">[HttpPost]
<span class="kwrd">public</span> ActionResult Create(Person person)
{
    <span class="kwrd">if</span> (ModelState.IsValid)
    {
        _people.Add(person);
        <span class="kwrd">return</span> RedirectToAction(<span class="str">"Index"</span>);
    }

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

The base Controller class that we inherit from also gives us some rich capabilities for evaluating the incoming parameter. We can inspect some simple pre-checked values, add validation error messages and deal with the user input as we see fit.

We are able to check ModelState.IsValid because we setup our model to require certain fields. You can add all kinds of validations to cover min and max values, ranges, matches based on Regexes, or, as we did, a check to make sure an Email address is valid. There are more still, and you can create your own if you like.

## Generating the View

Now, you’ve done this before but we’re going to approach it a little differently today. This time, when you right-click on the Create method in the controller, be sure to select the correct template and options.

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb11.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image23.png)

It’s a Create template, with the Person class as the model. You’ll want to clear the partial checkbox if it’s selected and make sure you “Include Script Libraries”.&nbsp; Remember that leaving the name of the view as Create allows the framework to pick it up on it’s own.

## This One’s Not Quite Free

The default view actually looks pretty good, in fact, you’d have to remove the form-horizontal style from the class attribute of the div (in the root of the form that is generated) to get the “standard” look-and-feel, which would be like this:

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb12.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image24.png)

But you’ll notice that we’re missing something in particular: a way to add skills to our person. Also, it looks way better with that form-horizontal in there!

## Getting Things Straight

So, if you removed that form-horizontal, add it back. Then we’re going to add the next little bit of markup, right before the submit button on the form:
<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="form-group"</span><span class="kwrd">&gt;</span>
    @Html.LabelFor(model =<span class="kwrd">&gt;</span> model.Skills, htmlAttributes: new { @class = "control-label col-md-2" })
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="col-md-10"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="input-group"</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="input-group-btn"</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">button</span> <span class="attr">class</span><span class="kwrd">="btn btn-default"</span> <span class="attr">id</span><span class="kwrd">="add-skill"</span> <span class="attr">type</span><span class="kwrd">="button"</span><span class="kwrd">&gt;</span>
                    <span class="kwrd">&lt;</span><span class="html">span</span> <span class="attr">class</span><span class="kwrd">="glyphicon glyphicon-plus"</span><span class="kwrd">&gt;&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;/</span><span class="html">button</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">span</span><span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">input</span> <span class="attr">type</span><span class="kwrd">="text"</span> <span class="attr">id</span><span class="kwrd">="skill"</span> <span class="attr">class</span><span class="kwrd">="form-control"</span> <span class="attr">placeholder</span><span class="kwrd">="Type, then click + to add"</span> <span class="kwrd">/&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">id</span><span class="kwrd">="skills-wrapper"</span><span class="kwrd">&gt;&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>

<span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="form-group"</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">div</span> <span class="attr">class</span><span class="kwrd">="col-md-offset-2 col-md-10"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">ul</span> <span class="attr">id</span><span class="kwrd">="skills-list"</span> <span class="attr">class</span><span class="kwrd">="list-group"</span><span class="kwrd">&gt;&lt;/</span><span class="html">ul</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">div</span><span class="kwrd">&gt;</span>
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

You’ll notice that we don’t actually have any new form elements here that will submit with the form (the input there doesn’t have a name and won’t appear in the submitted form collection), but we have a textbox that lets you enter some text.

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb13.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image25.png)

There is an empty DIV there that we’ll use to add hidden text inputs to the form via jQuery. We also have a styled UL list in there that we’ll use to display what has already been added.

We’ll add the following script to make that markup tick, which you can add at the bottom of the scaffolded script section in Create.cshtml:
<pre class="csharpcode">&lt;script&gt;
    $(<span class="kwrd">function</span> () {
        $(<span class="str">"#add-skill"</span>).click(<span class="kwrd">function</span> () {
            <span class="rem">// get the value of the added skill</span>
            <span class="kwrd">var</span> skill = $(<span class="str">"#skill"</span>).val();

            <span class="rem">// push hidden input to our form</span>
            $(<span class="str">"#skills-wrapper"</span>).append($(<span class="str">"&lt;input type='hidden' name='Skills' value='"</span> + skill + <span class="str">"' /&gt;"</span>));

            <span class="rem">// add list item for display purposes</span>
            $(<span class="str">"#skills-list"</span>).append($(<span class="str">"&lt;li class='list-group-item'&gt;"</span> + skill + <span class="str">"&lt;/li&gt;"</span>));

            <span class="rem">// reset the form</span>
            $(<span class="str">"#skill"</span>).val(<span class="str">""</span>).focus();
        });
    });
&lt;/script&gt;</pre>
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

The comments in the script lay out the goal here, essentially that we’re going to grab the value from the entry box and turn it into both a hidden form element and an LI in that unordered list we created earlier. Finally, we reset the input and set the focus.

Here it is in action!

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb14.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image26.png)

## I Really Like This Part

Remember how we didn’t add any form elements with the name “Skills”? Well, we did in our JavaScript. In fact, we’ll add as many _hidden_ form elements with the same name, “Skills”, to the form as the user would like. What happens to those form elements with the same name?

In your controller, in the POST method you added, set a breakpoint _anywhere_ in the code, then navigate to your Create page and fill out the form. When you submit, you’ll be able to inspect the Skills property of the person…the MVC Framework model binding is smart enough to see multiple instances of the same-named form element (in our case, a hidden text element) that has the name of a property in our model. 

It news up a collection for us and populates it with the values. Sweet! 

## Next Steps

While the form is now submitting and you can see any person you add in the collection, you wouldn’t know it unless you scrolled down on your index. Tomorrow we’ll add some more visibility into what is going on with Bootstrap alerts, assisted by a new base class for our controller and TempData.