title: MvcScaffolding Joy Ride with EF Code First
tags:
  - ASP.NET
  - MVC
  - MvcScaffolding
id: 1221
categories:
  - Code Dive
date: 2011-05-03 18:52:00
---

Okay, so here’s the situation: you are creating a web-based application that contains a series of tables, one of which has multiple one-to-one relationships with another table.

In your view, you’ll obviously have a series of drop downs that let you pick entities from the second table. This ensures data integrity, but is a bit of a PITA to wire up.

How do we put it all together?

### A Quick Overview

All this work is really not that bad with the latest tools refresh, and only takes a few lines of code.&nbsp; Let’s get there step by step and see how to make it happen.&nbsp; I’m going to make a Movie entity and a FavoriteMovieProfile entity.&nbsp; Here’s how she goes:

1.  Create a new ASP.NET MVC3 Internet Application.  <li>Use NuGet to install MvcScaffolding.  <li>Create a Movie class. POCO here, nothing special.  <li>Create a FavoriteMovieProfile class, with a bit of EF flavor.  <li>Scaffold our controllers with the repository pattern from the Package Manager Console.  <li>Extend the MoviesContext and create our own initializer for the database.  <li>Rescaffold the controllers. 

### Let’s Get Started – Project Setup

Okay, fire up Visual Studio 2010 and make sure you have the [latest bits](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=75568aa6-8107-475d-948a-ef22627e57a5). Create a new project and select the type ASP.NET MVC 3 Web Application. I named my project Movies.Web and called the solution Movies.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_2.png)

Create an Internet application, Razor view engine and HTML5, because that’s the in thing to do.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_4.png)

Go to the Package Manager Console and type the following:
<pre>Install-Package MvcScaffolding</pre>

This goes out to the interwebs and grabs the latest version of this toolkit, along with the required bits to generate the scaffolding.&nbsp; Going about its business, the Package Manager copies the packages to your project directory, updates your packages.config file and executes the required integration steps to light up the new references in your project.

Sweet.

### Creating the Classes

Okay, right click on your Models folder and create a new class called Schema. When the file opens, delete the class definition (I just want the file named schema). Add the following using statement at the top of the file:
<pre>using System.ComponentModel.DataAnnotations</pre>

We’re going to add two classes to the namespace, the first is Movie:
<pre>public class Movie
{&nbsp;&nbsp;&nbsp;&nbsp; 
public int MovieID { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public string Title { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public string LeadActor { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public string Director { get; set; }
}</pre>

Next, add in the FavoriteMovieProfile class like so:
<pre>public class FavoriteMovieProfile 
{&nbsp;&nbsp;&nbsp; 
 public int FavoriteMovieProfileID { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public string Name { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public int ComedyID { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public int HorrorID { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public int DramaID { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public int SciFiID { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public int FlickWithHalleBerryID { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 

[ForeignKey("ComedyID")] public virtual Movie Comedy { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
[ForeignKey("HorrorID")] public virtual Movie Horror { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
[ForeignKey("DramaID")] public virtual Movie Drama { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
[ForeignKey("SciFiID")] public virtual Movie SciFi { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
[ForeignKey("FlickWithHalleBerryID")] public virtual Movie FlickWithHalleBerry { get; set; }
}</pre>

You’ll notice a little bit of funkiness going on there, but it’s really not that bad once you get your head around it. Let me explain what’s going on.

EF Code First and the MvcScaffolding bits are good and smart, but they still need to know stuff.&nbsp; If we want our Views to light up correctly, MvcScaffolding needs to know how to render the model you’re trying to express.

You can boil the insanity down to a sort of key-pair model here. We need to store the id of the movie in the database but we also want to have the benefit of a rich model that knows about the movie entity being referenced.

Let’s look at just one key-pair sequence isolated from the rest:
<pre>public int ComedyID { get; set; }
[ForeignKey("ComedyID")] public virtual Movie Comedy { get; set; }</pre>

See, that’s not so bad, right!?

The ComedyID will be persisted to the database, while the Comedy movie object will not. We add the ForeignKey attribute for MvcScaffolding to clear up any confusing it might have mapping the object in its duties. Marking it virtual is a way of saying, “Hey, Mr. EF Code First groovy bits, please keep track of this object for me using the integer field I gave you, but when you load up my model, please include the full movie entity and not just a numerical field. K, thanks!”

Thanks, indeed, Mr. EF Code First groovy bits. Thanks, indeed.

### Scaffold Intending to Fail

The only reason I want to fail here is to illustrate a current limitation of Code First. We can build up our project very quickly here, so make sure you have a good feel around the solution if you’re new to this so that you understand what’s going on.

Back in the Package Manager Console, type the following:
<pre>Scaffold Controller Movie -Repository</pre>

This little gem treats you to a coffee break while it creates your CRUD capable controller, your repository, the DB context object you want and fleshes out your views.&nbsp; We’re going to do the same with FavoriteMovieProfile class now:
<pre>Scaffold Controller FavoriteMovieProfile -Repository</pre>

This does the same thing, but it respects that you already have a context in place and it just updates it as required.

Let’s run the project now and once it’s up, add a /Movies to your URL. The project will seem to run fine at first, but once you hit that movies view you should see this:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_7.png "image")

> The database creation succeeded, but the creation of the database objects did not. See InnerException for details.

And when we check the inner exception for details…

> Introducing FOREIGN KEY constraint 'FavoriteMovieProfile_Drama' on table 'FavoriteMovieProfiles' may cause cycles or multiple cascade paths. Specify ON DELETE NO ACTION or ON UPDATE NO ACTION, or modify other FOREIGN KEY constraints. 
> Could not create constraint. See previous errors.

Ah, yes, that failure I was talking about.&nbsp; So, here’s the deal. EF Code First doesn’t really have an elegant solution for several one-to-one mappings, or at least, not one that can be expressed with the fluent API or attributes alone. SQL Server will whine about trying to create the constraints, so we need to spoon feed it a bit.

I understand that spoon feeding in the future will be baked into EF Code First. Thankfully it’s not that big of a deal.

### A Side-Step to Clean Up

Code First means that we get our data model for free, without having to create a database. The schema is implied through our object definitions, the attributes we provide, and any additional directions we throw at it through the fluent API.

For me, because I’m not using an SDF, the database was created (or at least, it was attempted to be created) in SQL Server Express, which I have running locally. We need to clean that up a bit.

While we’re about to employ a method that will clean up the database independently and automatically, we had a fail here and the metadata required to track changes was not correctly written to the database. You will need to manually delete the database at this time. If you’re looking, it’ll be named ProjectName.Models.DataContext (with appropriate nomenclature, mine was Movies.Web.Models.MoviesWebContext).

Go ahead. I’ll wait for you here to finish.

### Extending our Movies Data Context

Once you remove the comments and add a little brevity to the model names, our MoviesWebContext class (found in the models folder) should look like this:
<pre>public class MoviesWebContext : DbContext
{&nbsp;&nbsp;&nbsp;&nbsp; 
public DbSet&lt;Movie&gt; Movies { get; set; }&nbsp;&nbsp;&nbsp;&nbsp; 
public DbSet&lt;FavoriteMovieProfile&gt; FavoriteMovieProfiles { get; set; }
}</pre>

We inherit from DbContext and define a couple of entity sets, each containing the models that we’ve scaffolded so far.&nbsp; To this class, we’re going to override a method from our base class and put in a bit of our own helpers to get that database created more smoothly.

<span style="font-family: courier new" face="Courier New">protected override void OnModelCreating(DbModelBuilder modelBuilder) 
{ 
&nbsp;&nbsp;&nbsp; // ... 
}</span>

In this method, we’ll add five lines like the following:
<pre>modelBuilder.Entity&lt;FavoriteMovieProfile&gt;()
   .HasRequired(m =&gt; m.Comedy)
   .WithMany()
   .HasForeignKey(m =&gt; m.ComedyID)
   .WillCascadeOnDelete(false);</pre>

You can see the mapping and likely get the idea. We mark the field as required and want to set up the foreign key, which requires the WithMany() call. This sets up a one-to-many relationship (which I mentioned above and will fix below) and also takes care of the cascade error with the false flag.&nbsp; All five lines are as follows:
<pre>![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_10.png "image")</pre>

Great, now that that’s out of the way, we have two small things to take care of: creating our own initializer and making sure it’s getting called at the appropriate time by our application.

### Initializing the Database the Cool Way

Well, not so cool if you actually have real data in there, I suppose, but I digress.

What we’re going to do now is to add one more class to our application (you can just put it in the dbcontext file for now) that inherits from <span style="font-family: courier new" face="Courier New">DropCreateDatabaseIfModelChanges</span>. This is a class that can automatically detect changes to the schema. When it does, it proceeds to drop the DB and recreate it. In this class we override the Seed method and clean up our one-to-one mappings.

The entire class looks like this:
<pre>public class MoviesDbInitializer : DropCreateDatabaseIfModelChanges&lt;MoviesWebContext&gt;
{&nbsp;&nbsp;&nbsp;&nbsp; 
	protected override void Seed(MoviesWebContext context)&nbsp;&nbsp;&nbsp;&nbsp; 
	{&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
	context.Database.ExecuteSqlCommand("ALTER TABLE FavoriteMovieProfiles ADD CONSTRAINT unique_Drama UNIQUE(DramaId)");
	context.Database.ExecuteSqlCommand("ALTER TABLE FavoriteMovieProfiles ADD CONSTRAINT unique_Comedy UNIQUE(ComedyId)");
	context.Database.ExecuteSqlCommand("ALTER TABLE FavoriteMovieProfiles ADD CONSTRAINT unique_HalleBerry UNIQUE(FlickWithHalleBerryId)");&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
	context.Database.ExecuteSqlCommand("ALTER TABLE FavoriteMovieProfiles ADD CONSTRAINT unique_Horror UNIQUE(HorrorId)");&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
	context.Database.ExecuteSqlCommand("ALTER TABLE FavoriteMovieProfiles ADD CONSTRAINT unique_SciFi UNIQUE(SciFiId)");&nbsp;&nbsp;&nbsp;&nbsp; 
	}
}</pre>

Basically, we’re just executing some SQL commands that take care of the part that EF can’t do on it’s own, creating our unique constraints.

Finally, drill into your Global.asax.ca file and add the following line of code in <span style="font-family: courier new" face="Courier New">Application_Start()</span>:
<pre>Database.SetInitializer(new Movies.Web.Models.MoviesDbInitializer());</pre>

(You’ll also need to add <span style="font-family: courier new" face="Courier New">System.Data.Entity</span> to your using statements.)

Press F5 and add the /movies to your URL to play along.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_13.png "image")

Add a couple of movies, then switch up your url to /FavoriteMovieProfiles for kicks. When you create a new one it’ll look like this:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_16.png "image")

…and when you save it and return to the grid, it’ll look like this:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/MvcScaffolding-Joy-Ride-with-EF-Code-Fir_A43C/image_19.png "image")

Very nice.&nbsp; Only thing missing is “melo” in front of “Drama”.

### Wrapping Up

This wasn’t hard at all, once we had the bits in place we needed.&nbsp; We are talking about less than 30 lines of code here, and this isn’t the simplest of models. Powerful stuff.

The ASP.NET MVC Framework is getting better with each iteration as it continues to add functionality, convention and integration points with external libraries, even those from the open source community.

The addition of NuGet to our toolkit, while not yet perfect, is still a huge help and makes adding third-party resources a breeze. 

The Swiss Army Knife of this trip is obviously the MvcScaffolding, which kicks out that boiler plate code that we’ve all come to be able to write in our sleep (but, obviously, it’s much more fun to have a tool do it).

And hey, I’m no tool.

To read more on this, visit:

*   [Morteza Manavi](http://weblogs.asp.net/manavi/default.aspx)<li>[Steven Sanderson](http://blog.stevensanderson.com)<li>[Scott Guthrie](http://weblogs.asp.net/scottgu/)<li>[Scott Hanselman](http://www.hanselman.com/blog/)

These guys all have MVC coverage and infos on MvcScaffolding.

Cheers.