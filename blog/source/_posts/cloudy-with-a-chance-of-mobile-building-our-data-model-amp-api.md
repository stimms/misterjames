title: 'Cloudy With a Chance of Mobile: Building Our Data Model & API'
tags:
  - Asp.Net MVC
  - Entity Framework
  - JSON
  - Windows Phone
id: 741
categories:
  - Code Dive
date: 2012-04-20 17:53:00
---

> Interested in having a web site that supports user-specific data? Perfect, use Asp.Net and the built in membership providers. Want to put it in the cloud? Sweet, jump on the Azure bandwagon. Want to easily define your data model and store it? No worries, EF Code First will make you cry, you love it so much. Want to access that data from Windows Phone? Well, my friend, you've come to the right place. If you want to follow along from the beginning, please check out the [first post in the series](http://oldblog.jameschambers.com/blog/cloudy-with-a-chance-of-mobile-mvc-framework-windows-azure-and-windows-phone). 

<span style="color: #666666" color="#666666">With my projects roughly set up the way I'd like them to be I can now start building my data model using Entity Framework or "EF".&nbsp; I'll use the model definition to quickly scaffold some UI and add some data to my project.</span>

<span style="color: #666666" color="#666666">I will then expose that data via the new Web API available in Mvc 4.&nbsp; Though I have two solutions set up, I'll only be working in the Mvc 4 project for this tutorial.&nbsp; A reminder that if you're opening the IDE to follow along after a break you'll need to open it up in Administrator mode for the Azure environment to run correctly.</span>

## <span style="color: #666666" color="#666666">Our Data Class</span>

<span style="color: #666666" color="#666666">To keep things simple I'm going to use a single entity class – and ultimately a single table – for this exercise.&nbsp; In the Mvc 4 project, use the Solution Explorer to locate and then right-click on the Models folder to add a new class called PhoneNumberEntry.&nbsp; The class will look like the following:</span>
<pre class="csharpcode">    <span class="kwrd">public</span> <span class="kwrd">class</span> PhoneNumberEntry
    {
        <span class="kwrd">public</span> <span class="kwrd">int</span> PhoneNumberEntryId { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> FirstName { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> LastName { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> PhoneNumber { get; set; }
        <span class="kwrd">public</span> <span class="kwrd">string</span> UserName { get; set; }
    }</pre>

<span style="color: #666666" color="#666666">Hey, don't judge. I said I was keeping it simple. ![Winking smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/wlEmoticon-winkingsmile_2.png)</span>

<span style="color: #666666" color="#666666">Next, I need to tell EF how to create the database, namely, I have to tell it what tables to create and of which entity. I do this through inheritance and attributes on our classes.&nbsp; You can put this definition in the same file as the PhoneNumberEntry class.&nbsp; Again, it's very light weight.</span>
<pre class="csharpcode">    <span class="kwrd">public</span> <span class="kwrd">class</span> ContactContext: DbContext
    {
        <span class="kwrd">public</span> DbSet&lt;PhoneNumberEntry&gt; PhoneNumberEntries { get; set; }
    }</pre>

<span style="color: #666666" color="#666666">Perfect, now I'm rolling. To create the UI, I'm simply going to use the tooling that's built into the MVC Framework through Visual Studio 2010\. In order to fully leverage this, I need to compile my classes so that the IDE can reflect on my project and show me a list of them.&nbsp; I like to use the keyboard shortcut Shift + Ctrl + B to build. </span>

## <span style="color: #666666" color="#666666">Building the Views</span>

<span style="color: #666666" color="#666666">With my project compiled, I right-click on the Controllers folder in Project Explorer and select Add –&gt; Controller. I name my controller appropriately, select my PhoneNumberEntry class and name a new Data Context Class.&nbsp; I choose the Controller with read/write actions and views, using Entity Framework as the template.</span>

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_3.png "image")

The tooling builds my controller and scaffolds out my views for me. 

I only want members who have registered to create entries, and I want to each user to have their own list. I created a UserName property on my model, so I now need to deal with that field on my own. In my Create and Edit views, I remove the UserName field. Here's an example of what that HTML looked like before I removed it:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_6.png "image")

With that out of the way (in both Create and Edit) I need to update my controller accordingly. I open PhoneNumberEntryController in the Controllers folder and add the Authorize attribute to my class. 
<pre class="csharpcode">    [Authorize]
    <span class="kwrd">public</span> <span class="kwrd">class</span> PhoneNumberEntryController : Controller
    {
        <span class="rem">// ...rest of class here</span>
    }</pre>

By attributing the controller in this way (at the class level), I am telling the Asp.Net runtime that any user who accesses any action on this controller needs to be logged in using the authentication scheme of the site.&nbsp; Now, all users who try to navigate to any of my Phone Numbers pages will first be prompted to log in.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_9.png "image")

## Driven By the Logged in User

Next, I want to make sure that the user only ever sees the phone numbers that they create.&nbsp; The index method needs to be adjusted to only return the appropriate entries.
<pre class="csharpcode">    <span class="kwrd">public</span> ActionResult Index()
    {
        <span class="kwrd">string</span> username = User.Identity.Name;

        var phoneNumbers = db.PhoneNumberEntries
            .Where(p =&gt; p.UserName == username)
            .ToList();

        <span class="kwrd">return</span> View(phoneNumbers);
    }</pre>

In the Create and Edit methods decorated with the HttpPost attribute (these are the methods that accept the PhoneNumberEntry parameters from the form) I add a bit of code to capture the user's login name and store that in the model.
<pre class="csharpcode">    [HttpPost]
    <span class="kwrd">public</span> ActionResult Create(PhoneNumberEntry phonenumberentry)
    {
        <span class="kwrd">string</span> username = User.Identity.Name;
        phonenumberentry.UserName = username;

        <span class="kwrd">if</span> (ModelState.IsValid)
        {
            db.PhoneNumberEntries.Add(phonenumberentry);
            db.SaveChanges();
            <span class="kwrd">return</span> RedirectToAction(<span class="str">"Index"</span>);
        }

        <span class="kwrd">return</span> View(phonenumberentry);
    }</pre>
> **Important note**: There are many more security considerations to take and I am only demonstrating some bare minimums. In a real-world scenario you would use a repository outside of the controller and would do the appropriate checks to protect your data at various levels of your project. Particularly, any code to retrieve or modify the data should have checks in place to ensure the user is authorized to view or modify the resource in question.&nbsp; As an exercise to the reader, work through the PhoneNumberEntryController and firm up the user access/rights to a better degree.

Everything should be set here to capture some data, but I want to be able to easily access these bits of UI.&nbsp; I open up my _Layout.cshtml file, located in Views\Shared in the Solution Explorer and modify the snippet of code that creates the menu elements.&nbsp; When finished, it looks like so:
<pre class="csharpcode">    &lt;nav&gt;
        &lt;ul id=<span class="str">"menu"</span>&gt;
            &lt;li&gt;@Html.ActionLink(<span class="str">"Home"</span>, <span class="str">"Index"</span>, <span class="str">"Home"</span>)&lt;/li&gt;
            &lt;li&gt;@Html.ActionLink(<span class="str">"About"</span>, <span class="str">"About"</span>, <span class="str">"Home"</span>)&lt;/li&gt;
            &lt;li&gt;@Html.ActionLink(<span class="str">"Phone Numbers"</span>, <span class="str">"Index"</span>, <span class="str">"PhoneNumberEntry"</span>)&lt;/li&gt;
            &lt;li&gt;@Html.ActionLink(<span class="str">"Contact"</span>, <span class="str">"Contact"</span>, <span class="str">"Home"</span>)&lt;/li&gt;
        &lt;/ul&gt;
    &lt;/nav&gt;</pre>

The LI elements are formatted through the default site template and the UL is used as a wrapper to generate the menu at the top of the page. The ActionLink helper method renders an anchor tag for the specified controller/action.

## Getting Some Data In There

Before I move onto the API portion of this exercise, I press F5 to run my site and use the newly created Phone Numbers menu to get at the UI the framework generated for me.&nbsp; I add several entries as you can see below.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_11.png)

Now, I want to go one step further. This is about accessing the data for a specific user, so I want to have some other data in there as well to prove out that we can isolate the data and make use of the Authentication providers in Asp.Net.&nbsp; I create another user and add more entries as I did above, and you can see in the database now how there is data in there for two users. 

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_thumb_4.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_13.png)

I now have data specific to a couple of users and can build out the service piece.

## Cutting a Web API Controller

This piece is really easy and uses the tooling and templates from the MVC Framework.&nbsp; I add a new folder called Api to my Controllers folder and then right-click on the Api folder and select Add –&gt; Controller.&nbsp; I name it PhoneNumberEntryController and select the Empty Api controller as the scaffolder. Even though it's the same name, it ends up in a different namespace so everyone's happy.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_thumb_5.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_15.png)

Much like the controller returning a view, I want the ApiController to only allow authenticated requests, and I want the method to return only phone numbers for the current user.
<pre class="csharpcode">    [Authorize]
    <span class="kwrd">public</span> <span class="kwrd">class</span> PhoneNumberEntryController : ApiController
    {
        <span class="kwrd">private</span> CloudyWebSiteContext db = <span class="kwrd">new</span> CloudyWebSiteContext();

        <span class="kwrd">public</span> ICollection&lt;PhoneNumberEntry&gt; Get()
        {
            <span class="kwrd">string</span> username = ControllerContext.Request.GetUserPrincipal().Identity.Name;

            var phoneNumbers = db.PhoneNumberEntries
                .Where(p =&gt; p.UserName == username)
                .ToList();

            <span class="kwrd">return</span> phoneNumbers;

        }
    }</pre>

There's one more thing I want to do: I want to suppress the Xml formatter that is on by default in WebAPI.&nbsp; By turning it off, the only remaining default serializer is Json.&nbsp; Glenn Block [shows us](http://codebetter.com/glennblock/2012/02/26/disabling-the-xml-formatter-in-asp-net-web-apithe-easy-way-2/) the most performant way to do this in our Global.asax's Application_Start menthod:

> <pre class="csharpcode">GlobalConfiguration.Configuration.Formatters.XmlFormatter.SupportedMediaTypes.Clear();</pre>

People. We are now awesomely rocking.&nbsp; I will coin the term "awesrocking" because of the awesomeness here.

## ![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/image_18.png "image")Testing Out the Api

This is the part where I could start to hit some roadblocks if I didn't have the right tools. I love Fiddler and use it first whenever I'm, uh...fiddling around, but I can't use Fiddler to poke around the API because I _need_ a user to see what I'm doing and authentication is forms based in this context. I can't use IE efficiently because every time I get a Json document returned it prompts me to open the file of an unknown file type. I could use Notepad here (or a [registry hack](http://stackoverflow.com/questions/2483771/how-can-i-convince-ie-to-simply-display-application-json-rather-than-offer-to-do)), I know, but it's a pain and I like to see my data all nicely formatted ![Winking smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Cloudy-With-a-Chance-of-Mobile-Building-_DC61/wlEmoticon-winkingsmile_2.png).

FireFox to the rescue, along with a little helper plugin called JsonView.&nbsp; In Visual Studio 11 this will be even easier because I can choose the browser to launch, rather than launching a separate browser or setting the OS default.&nbsp; Regardless, I run the app in FireFox, log in to my site, then I hit http://127.0.0.1:81/api/phonenumberentry to see the data. JsonView takes over and I see my phone numbers as we expect.

You can see the result in the image to the right.

## Wrapping Up and Next Steps

Okay, here's the quick breakdown for the steps we have taken to this point:

1.  Added our model (class)<li>Added our data context<li>Scaffolded our views<li>Secured our views<li>Tied the data entry and loading to specific users<li>Added some data<li>Created a Web API controller<li>Secured the controller and started returning the correct data

&nbsp;

With these things in place, I can now start work on my phone client. Our project is already set, so we can get to the heavy lifting right away! Stay tuned for the next post where I'll get the phone client lined up to consume the data and display it.&nbsp; Later on we'll talk about some things we can do to better improve the code and get us closer to best practices.