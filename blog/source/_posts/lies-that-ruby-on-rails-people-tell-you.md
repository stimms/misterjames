title: 'Myths That (Some!) Ruby on Rails People Tell You'
tags:
  - Asp.Net MVC
  - Make Code Not War
  - Ruby on Rails
id: 511
categories:
  - Code Dive
  - Develop Meta
date: 2012-10-16 17:31:00
---

> Hey...this post just shot past 10,000 views and I've received a ton of feedback that the title was inflammatory, which is completely fair. I've renamed it from "Lies that Ruby on Rails People Tell You" to the above, as I realize I was painting with the same wide strokes I am complaining about in this post. Thanks to all who were calling me on my BS title.&nbsp; Cheers, -JC 

Over the course of the last several years I've had the pleasure of working with developers from many different backgrounds.&nbsp; It is true that there is incredible talent in every discipline. It is also true that you can build out great solutions on any of today's modern frameworks.

I got my legs programming when it was cool to hate Microsoft (the first time around).&nbsp; I built CGIs that ran on Linux servers, attended "distro install competitions", ran my home internet off of the Linux Router on a Floppy project and through it all maintained that the blue mothership from Redmond was evil.

Then I grew up.

The reality of a multi-platform solution hit me very early in my career. It was great to be part of a production environment where many different engineers took on projects of different capacities requiring multiple skillsets and a ton of teamwork. I had learned that the "right way" to do something _was_ through teamwork, cooperation and ingenuity, and that a particular breed of technology or the language you chose played a much smaller role in a successful project.

For over a decade now I have been programming primarily on the MS stack, and in the last three years have spent a lot of time working with the MVC Framework. I enjoy the workflow, the toolset, the community and especially the opportunities I've been granted that allow me to share my experience and mentor others.

Today, it is the attitude I once held – that the way to do all things is the way I do it with the technology I prefer – it's that attitude that frustrates me more than anything. It really is true that the things you like least in others are the things you like least about yourself.

## Okay, So What Am I Trying to Do Here?

Let's start with what I'm _not_ doing. I am _not_ trying to say that the MVC Framework is better than Ruby on Rails.&nbsp; That may be my opinion, but that's because it's _best for me._ Please appreciate the difference.

What I _am_ trying to do is to show that there is much less of a gap than might be suggested. The MVC Framework is mature, stable, performant, and worth exploring. It is also very likely worth switching to if you are on older tech.

What I am _also_ trying to do is to grow the chorus of folks who are willing to try new things out. This is the one I hope I win at, because this is how things are going to get better. I often use the analogy of the iPhone, Android and Windows Phone. We have three _great_ options for smart phones because there are three vendors taking a serious whack at the cat.

## Oh Ruby, Don't Take Your Love to Town

While I've had the privilege of working with some of the brightest minds along the way, I've also been part of teams where being at odds over which tech to use ends in turmoil on the project. Throughout my career the arguments have varied and blurred in focus between operating systems, languages, platforms, patterns, frameworks or even companies.&nbsp; Windows vs. Linux, c++ vs. Java, Mac vs. PC, iPhone vs. Android, AWS versus Azure, .Net vs. [insert any framework here].&nbsp; Some battles seem to be more intense than others, but in the last year I discovered one that actually blindsided me, I was completely unaware it was brewing. 

And it comes from Ruby on Rails developers.

# Judging Books

I want to be fair here – there's more than one camp in the Rails world and I've met some insanely intelligent, productive, passionate developers who could do pretty much anything for mobile and web in RoR.&nbsp; I've worked alongside some that have a background using .Net.&nbsp; But there's a slice of the Railers that has either never used the MVC Framework or used it in an early incarnation and have since not only condemned the thing, but also the developers that do good things – intelligently, productively and passionately – working inside Visual Studio.

More recently, I've been surprised by the venomous bile that has built up in some of these folks' throats over the last year, to a point where they are spreading inaccurate, misleading and even entirely made up points to counter what is offered in the .NET camp for MVC development.&nbsp; So I want to set the wheels in motion to correct some of these myths.

## Myth 1: It's Easier to Bootstrap a Project In Rails

It's quite easy in Rails to get started. If you are comfortable with the command line you can create a new project by invoking a couple of commands. You can use template sites or boiler-plate generators that allow you to select the most common elements and assemble a project to get started.&nbsp; There are also UI-based tools that facilitate a basic template of a project in Rails to get you kickstarted.

But that doesn't make it easier.&nbsp; Just different.

As an MVC Framework developer, I also get my choice of options. To start with, I can just say "File-&gt;New-&gt;Project". I can use default project templates that are included in Visual Studio, download starter projects from the web, or, if I prefer, start with a completely empty solution and use Nuget to cherry-pick functionality that other developers have made available. 

## Myth 2: It's Easier to Manage Packages Rails

This one was true, especially a couple of years ago. But Nuget – the open source package manager for Visual Studio – has come a long way and as of this writing there are very few things that it lacks. You can manage packages at the solution or project level. You can leave your binaries out of source control and leverage automatic package restore. You can cache projects at the machine or solution level. You can (very easily!) build out your own Nuget server, or use a third party solution like MyGet to host widely available private package feeds or an internal corporate feed.&nbsp; Semantic versioning, dependencies, package upgrades and more are all implemented.&nbsp; 

Furthermore, the Package Manager Console is built on top of PowerShell and can be extended by Nuget Packages, meaning other developers can push out new functionality to your IDE along with functionality for your project by adding new commands to the Console.&nbsp; It also has access to (almost) the entire extensibility points that Visual Studio offers, so it can modify your configuration, add files, create references, or insert code and more to affect your project.

And, especially for .Net developers, there are additional benefits.&nbsp; Nuget is not limited exclusively to MVC projects; rather, you can use it for console apps, web service projects, Windows 8 or Windows Phone projects or whatever you like, so it's opened the door to even more sharing and open source community to more devs around the world.

## Myth 3: It's Better to Develop with RoR Because it's Open Source

If your personal opinion or corporate agenda is that it's "better" to develop on OSS solutions, then good on you. For the record, the MVC Framework is completely open source. So is the _entire_ Asp.Net stack. And the script libraries that are included in the project templates.&nbsp; Oh, and Nuget, too.&nbsp; This is not a differentiator for Rails.

## Myth 4: I Get More Options for View Engines/I'm not Limited by Microsoft View Engines

First of all...what limitations?&nbsp; The Razor view engine – the default for MVC – is likely one of the best view engines around right now. It did not have to support years of history when it was crafted, but it got to learn from it. It is intelligent _as all get out_ and understands inline switching between code and markup. It is darn fast. It is terse (though not as much as Haml, for example) and versatile. 

There are other view engines available to the MVC Framework such as Spark, NVelocity, Brail, or others or you can write your own.

Yes, in RoR there are many view engines. But like the MVC Framework there is a default, a whole bunch of less relevant engines and niche ones that exist to satisfy niche requirements.&nbsp; Several people have demonstrated why they _prefer_ certain ones over others, but no one has shown me why their view engine of choice is _better._

## Myth 5: Ruby on Rails has Been Around Longer and is More Mature

This is an irrelevant, misleading comment that is usually followed by statements like "there are more answers to questions on Stack Overflow" or "there is a better community" or the like.

Let's address the first point: Ruby on rails has indeed been around longer than the MVC Framework, but this is a slight of hand.&nbsp; The MVC Framework is built on top of the .NET Framework and the toolset for MVC is an extension of Visual Studio.&nbsp; Maturity isn't dictated by age, and what is really important is how comfortable you are with your tools.

I know, right away, that comments will fly like "Visual Studio is a memory pig and crashes all the time" or "Sure, I like VS, it's the best wrapper for ReSharper that's out there".&nbsp; Fine, if you prefer a different environment that is your choice, but it doesn't mean you can dismiss VS for what it is: the software development tool used by more professionals than other other IDE on the planet (I can't speak for the moon).&nbsp; I use it every day with very little friction and the productivity wins are quite large. 

On the second point: Stack Overflow may have more questions about Ruby. But let's not forget that more people asking questions might not be a "good" thing! Let's pause and think _why_ people might be asking questions for a moment. Are they relevant for recent releases? What are the quality of the answers?&nbsp; Or the questions?&nbsp; And if your party line is that more questions are better, then head over to the Microsoft forums where there will be questions on MVC, ASP.NET, c#, Visual Studio, or the .Net Framework itself.&nbsp; You're likely to find just as many bad questions, answers, arguments and features in any language, if you look hard enough.

## Myth 6: Ruby Tooling Provides Better Database Practices

I will be quite frank: until EF Code First had migrations, it was a great little hack to use in presentations and show how fast a developer I could be. Even in the earlier versions of EFCF migrations there were struggles to understand targeting, versions, naming, which connection strings were being used, etc.

But today's database tooling is much better, and the MVC Framework aligns quite well with corporate environments and software professionals around the world.&nbsp; Everything you need for automatic DB generation is built into the default MVC project templates with no additional requirements or dependencies to add.

But if you don't want to use Entity Framework, how do you access the DB in the MVC world? However you want to. That's abstracted away so that you can have SQL, SQL Express, My SQL, Oracle, or CSVs files retrieved via JSON served from a Commodore 64.&nbsp; It doesn't matter what you choose.&nbsp; You can pick whatever ORM you like.

And if you do choose EF Code First, then I'm happy to say that migrations have finally arrived. You can use choose your strategy for database creation, seeding or migrating up and down, or use the default (and quite capable) behaviour. You can enable settings that allow for very rapid prototyping or strict it up to prevent nilly-willy changes.&nbsp; Your "plain old CLR objects" or POCOs are used to infer your naming, relationships, indexes and defaults, but you can override this behaviour as well.&nbsp; Objects and properties are strongly typed, navigation across entities is discoverable as you code and much of what is there will be familiar to experienced developers.

Besides, what do you do in a RoR application if you have to use legacy data?&nbsp; Or if corporate requirements dictate the inclusion of Data Architects on your project? You have to slog code, that's what.

Again, this is an area that has virtually been eliminated as a talking point when it comes to comparing the frameworks.

## Myth 7: It's More Clear Where Things Go in a RoR Project

Disagree completely. You may be more familiar with where your bits need to be flipped, but this is just an issue of groove.&nbsp; Once you know where things are – in either MVC or Ruby on Rails – you build a comfort level with your tools, classes, scripts, models, packages, etc. and you get to work.&nbsp; You may prefer your toolset over another but let me assure you that I don't stumble around in my projects trying to find the parts I'm going after.

I know exactly where my models go.&nbsp; And my views.&nbsp; And helpers.&nbsp; And filters.&nbsp; If you come from the Ruby world and you don't know where these things go, I can also assure you that the directory structure is no less intimidating in RoR projects when you aren't familiar with the layout, patterns or best practices.&nbsp; This is _exactly_ why we learn our tools, languages and environments: so that we can be productive.

I have had live conversations with the actual humans (yes, they are humans!) who have built out Razor, the MVC Framework, Nuget and who have otherwise been big contributors to the ASP.NET landscape.&nbsp; They are just as concerned about building out a great development experience as anyone else out there building tools.

## Myth 8: It's Easier to Target Any Environment I Want

One of the great things about Ruby on Rails is the ability to just create a copy of environment you're working with – say your test environment – rename it and then simply alter the configuration elements you need changed for the new one.

One of the great things about solutions in Visual Studio is the ability to just create a copy of environment you're working with – say your test environment – rename it and then simply alter the configuration elements you need changed for the new one.

See what I did there?

# Let's Work Together and Build Awesome

Look, what we really need to do is cross-train. Seriously.&nbsp; There are _great _things in the RoR world, and there are _great_ things in the .Net world.&nbsp; They argument that "well, they just copied x from y" when you are talking about any framework is pointless.&nbsp; I can't see how any professional would argue otherwise.

There are many different shapes of hammers; they all pound nails.

When we accept that – and learn from each other, rather than beating each other up – then we can challenge and inspire each other and push our craft much further.&nbsp; You can't say you're a better developer than me because you've memorized your syntax, and I can't say I'm the better dev because I'm faster with IntelliSense.&nbsp; We're different.

And, man, that's just plain.Okay().

# Steps v.Next

If you are a Ruby on Rails developer, I highly encourage you to grab the free tooling and [try on some Mvc](http://asp.net/mvc).

And to my fellow .Netters, go rake yourself [some Ruby love](http://rubyonrails.org).