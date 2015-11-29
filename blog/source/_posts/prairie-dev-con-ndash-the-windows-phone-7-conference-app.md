title: Prairie Dev Con - The Windows Phone 7 Conference App
tags:
  - PrairieDevCon
  - Windows Phone
id: 901
categories:
  - Conferences
  - Projects
date: 2011-11-15 18:13:00
---

I have published my version of the [Prairie Dev Con](http://www.prairiedevcon.com) conference app out to the Windows Phone Marketplace, which you can browse or invoke an install from [here](http://www.windowsphone.com/en-US/apps/8e74279f-c4ab-43a6-bff3-a1a4ba1a8514). I am going to write a little at a high level about the experience, then get into some meatier posts down the road.

# Timeline

It took ten days to build. Actually, it was more like two flights, a few hours in TO Pearson airport, one late night and an after-supper-code-crunch. I wish I had used some more agile tricks and principles to help pull through this project (some great conversations with [Steve](http://twitter.com/#!/srogalsky) and [David](http://twitter.com/#!/davidalpert) over the last few months about Agile have been a solid influence).&nbsp; It certainly helped that [David](http://twitter.com/#!/davidalpert) and [Amir](http://twitter.com/#!/abarylko) had already built the JSON services, so my responsibilities were simply in being a consumer.

# ![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Prairie-Dev-Con--The-Windows-Phone-7-Con_871C/image_3.png "image")The Tech That Made it Tick

*   MVVM Light Toolkit – this was a God-send. Seriously, it made visual binding in Expression Blend so easy. I put my time in the trenches hand-bombing XAML in the past. I think everyone should know XAML inside and out if you want to build WP7 apps, but once you do...man alive. Laurent, I'm buying you a drink!  <li>Silverlight Toolkit for Windows Phone – This gem adds a bunch of controls to the designer's tool chest.&nbsp; I made use of the page transitions and the hub tiles. I wasn't able to get a good thing going with the context menu - I was trying to use it on data-bound items and there was too much friction there to use it without pain.  <li>SQL CE and LINQ – Yum. Though I had a few struggles with many-to-many relationships (I'll explain later) this makes working with relational data so simple. I was going to go with custom serialization but at the last minute threw together a spike solution and committed.&nbsp; It was also good to exercise some of the Mango functionality.  <li>JSON.net – This little library chews through JSON data and does an excellent job of deserialization without object mapping.  <li>Visual Studio 2010 – Best development environment on the planet. Period.  <li>Expression Blend – As I mentioned, I've earned my stripes in raw XAML, so I have no problem using the right tool for the right job. While the markup it creates is still a little verbose (and you have to cajole it into using best practices) Expression Blend makes such quick work of templating, databinding and making your app look like a big bowl of awesomesauce. With cherries.  <li>Marketplace Test Kit – this is a handy utility that plugs into VS2010 when you install the phone SDK. It has some automated tests, makes sure your images are in check, then gives you the skinny on what you need to do to get your app approved.&nbsp; This is the kind of _brutal transparency _that app developers deserve and other vendors (*cough* Apple *cough*) would do good to imitate. 

&nbsp;

This was a fun app to build. I got to touch (and dive into) a good variety of technical elements. I'm a little sick in the head, in that I like when things break. I tend to find that when they do break you get that exciting rush of running down the rabbit hole to see what makes things tick.

# Friction Points – Things I Don't/Didn't Like

## Navigation and User Experience

The back stack of NavigationService helps to maintain a consistent feel across the whole UI, across the whole phone. Microsoft is fighting for this, recommending this and I agree with it for the most part. In my app, though, I ran into this UX problem; consider the following:

*   User clicks on the speakers tile  <li>They click on a speaker  <li>They click on a session  <li>They save the session (which navigates to the sessions page again)  <li>They go to another session  <li>They click back (which returns them to the session list) 

Now, to get to the "home" screen, they still have to click back 4 times.&nbsp; Which is gross.&nbsp; I "solved" this by adding a "return" button in the application bar of the session list, but I want to study this more and see if it's a design problem.&nbsp; The thing is, until you want to move backwards, the app seems easy and fluid to move through speakers and sessions and I don't want to break that.

## Many-to-Many Relationships in SQL CE

Another hang up (for a couple hours, on the plane with no internet access, boo!) was with the many-to-many relationships in SQL CE. I have "sessions" and "tags". Tags have several different sessions and sessions can have more than one tag.&nbsp; In the mobile version of code-first, you have to be more explicit about these relationships, something I missed originally.&nbsp; I'll put a post together on this.

## Blend and Build Configurations

This one is completely obvious once you get what's going on, yet will make you pull out your hair if you don't. And I don't have a lot of hair to begin with.

I was switching between Release, Debug and a config I created called "Trial".&nbsp; Once you get in the pattern of flowing back and forth between VS 2010 and Blend, you have to remember that Blend only reads from your Debug directory. For example, if you're building in Release config changing parts of your ViewModel in VS and then flipping over to Blend to wire up the UI, the changes just don't show up when you're trying to bind.&nbsp; Just be aware of this, if you're using multiple build configurations.

Again, completely obvious, just not at 2 in the morning.

## My Own Threading Design

I rushed this piece and didn't think this through. As a result, I had to create a bunch of events rather than using existing messaging facilities and even then, I didn't get the UI responsive. I'm redesigning this piece.

# Awesomeness – Things that Worked Really Well

## MVVM Light Toolkit and Expression Blend

I can't stress enough how quickly things come together when you're using "Blendable" view models.&nbsp; This app was very much an experiment as to where those pieces fit and where code should live (as evidenced in the source), but visualizing data is just so easy with this marriage.

## JSON.Net

While I originally missed the LINQ-to-JSON support, I've started playing with this and it's great. In my current build I had to smack together a bunch of DTO objects that could easily be used by JSON.Net, then I crafted up my real model objects and did a mapping.&nbsp; I know now about the LINQ support and will be using that to replace my hackiness in my psuedo-DAL

## Visual Studio 2010 and the Mango SDK

Folks, if you're developing on any other mobile platform, your tools aren't as good. I don't rightly care if that offends you. I've used the iOS and Android toolset (even with folks more well versed than I) and they've not shown me a better end-to-end experience. I've put up a challenge on Twitter for Android developers to do a head-to-head tool comparison at a conference with no bites.&nbsp; I'm talking everything from New Project to the emulator all the way through to the Marketplace Readiness test kit.&nbsp; It's all there, and I can effortlessly switch between Blend and VS.&nbsp; The SDK is strong, close to the metal, and fully integrated into Visual Studio. It's what it should be.

# And, The Project is Going Open Source!

That's right, even in it's half-baked bits right now, I'm pushing it up to Git.&nbsp; [Amir](http://twitter.com/#!/abarylko) has promised that the code will not be used to judge me ![Winking smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Prairie-Dev-Con--The-Windows-Phone-7-Con_871C/wlEmoticon-winkingsmile_2.png) so I'm trusting that he'll hold true to that. There are no tests, the view models are inconsistent, I'm not making full use of the JSON tools and my templates are crafted quite differently from one view to the next.&nbsp; Simply put: This code will not appear in it's current form on my resume ![Winking smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Prairie-Dev-Con--The-Windows-Phone-7-Con_871C/wlEmoticon-winkingsmile_2.png).

# And For You, the Reader

Well, surf over [here](http://www.windowsphone.com/en-US/apps/8e74279f-c4ab-43a6-bff3-a1a4ba1a8514) and try it out! I would love feedback on what conference attendees would like to see in an app.

Equip yourself with [the dev tools](http://create.msdn.com/en-ca/home/getting_started) from MSDN.

Familiarize yourself with [JSON](http://www.json.org/).&nbsp; Grab a copy of [JSON.NET](http://json.codeplex.com/).

And start building apps!