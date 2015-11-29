title: Regina is Hacking Awesome
tags:
  - Azure
  - HackAThon
  - Windows Phone
id: 581
categories:
  - Code Dive
  - Conferences
  - Projects
date: 2012-09-29 17:38:00
---

![](http://hackdays.ca/wp-content/uploads/2012/04/HackDays.png)

Yesterday I had the pleasure of participating in HackRegina, an event organized by [Chad McCallum](https://twitter.com/ChadEmm) and supported by [HackDays.ca](http://hackdays.ca) with a ton of sponsors.&nbsp; There were about 40 people who came out to build apps and hack together solutions to solve the world's problems. 

Coming with neither a solid idea, nor a team, I was lucky enough to partner up with [Johannes Lindenbaum](https://twitter.com/jlindenbaum). I think we actually ended up going with the first idea that we threw out there and just pushed through on a "this train must stay on the rails or we will not finish our project" track.&nbsp; 

And the end of the day, Johannes and I put together a complete geo-location based game called **DiscovR**, focused on the City of Regina, and ended up winning the prize for "Best Windows Phone". We took home some loot to boot.&nbsp; If you didn't read that last sentence too carefully, I'm fully geeking out over the fact that we built a whole game in about 6.5 hours.

## The Idea

Here is the basic idea: players compete for points, awarded for being the first to explore part of Regina. Each day, a new "goal" is posted to the game's backend, and the race is on to check in at the new location and capture the most points.&nbsp; After you've checked in, you get a chance to see all the local restaurants and pubs (based on your location).&nbsp; Each check in reduces the points left for the next person who checks in, so the fastest onsite gets to climb the leaderboard the fastest.

All of this in the name of exploring the City of Regina.

## The Build

[![SplashScreenImage](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/SplashScreenImage_thumb.jpg "SplashScreenImage")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/SplashScreenImage_2.jpg)In a nutshell, here's all the APIs, SDKs, libraries and tools that we used to get the game running:

*   [Windows Azure Mobile Services](http://www.windowsazure.com/en-ca/)
        *   This is how we got up and running quickly with our backend. We were saving game information within 30 minutes of starting our project.&nbsp; All our REST end points were automatically generated for us and WAMS takes care of all the CRUD operations for us. <li>[Eclipse IDE](http://www.eclipse.org/downloads/moreinfo/java.php)
        *   The best native app development environment for building Android apps. <li>[Android SDK](http://developer.android.com/sdk/index.html)
        *   Used to build the game client (used to play the game). Provides development libraries, an emulator and deployment tools for Android devices. <li>[Visual Studio 2010](http://www.microsoft.com/visualstudio/eng/whats-new)
        *   Building apps for .Net, Windows or Windows Phone, this is just what you use. <li>[Windows Phone SDK](http://www.microsoft.com/en-ca/download/details.aspx?id=27570)
        *   Helped us build the game manager, used to set new goals for the game players. Provides development libraries, an emulator and deployment tools for Windows Phone. <li>[Node.js](http://nodejs.org/)
        *   I wrote the game rules on the cloud, hosted on Azure. There's a layer to Azure Mobile Services that lets you intercept the CRUD operations to incorporate security, push notifications, dynamic model composition and more. All your server side scripts get to leverage Node. <li>[SQL Azure](http://www.windowsazure.com/en-ca/)
        *   This is the data store that backs Windows Azure Mobile Services. For the most part, we didn't even need to remember it was there, but when it came to resetting the database before our presentation, we were able to connect with SQL Management Studio and just execute the appropriate queries to clean things up. <li>[City of Regina Open Data API](http://openregina.cloudapp.net/)
        *   A municipal government that gets it – or is at least starting to.&nbsp; Great on them for making a ton of data available for free, public consumption. <li>[Yellow API](http://www.yellowapi.com/)
        *   This is where we got our data for exploring the neighbourhood after you've checked in for your points. <li>[Fiddler2](http://www.fiddler2.com/fiddler2/)
        *   A comprehensive tool that lets you compose and deconstruct HTTP messages.&nbsp; We used this – paired with dynamic schema in WAMS – to build out our first JSON messages and layout our data model. <li>[Silverlight Toolkit for Windows Phone](http://silverlight.codeplex.com/)
        *   A great little suite of components that help to make great looking apps for Windows Phone. 

&nbsp;

Whew. Looking at that list, it's hard to believe we built the game in less than a day. But it sure validates my statement about having no room for things going wrong!

## What Went Wrong

[![hubtiles](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/hubtiles_cc6f0e54-8f88-429c-81a2-26f38c5bad6e.png "hubtiles")](http://openregina.cloudapp.net/)We made smart choices early and spent some time thinking about the path of least resistance, and for the most part this served us well.&nbsp; There were a few hiccups along the way.

*   Android not playing nicely with HTTPS  <li>Windows Phone not reporting back meaningful error messages related to networking  <li>City of Regina "Point of Interest API" is a great start, but it provides no rich content about _why_ the point is interesting  <li>Yellow Pages API gives you data like it's 1999.&nbsp; Seriously. Like, they wrote some XML in 1999 and then just wrapped curly braces around it and called it JSON.  <li>OAuth. I don't need to say any more than that.  <li>OData web services wrap results in a "d" property...so you if want to deserialize the data you have to create an object representing the response, then have a property called "d" which is an array of the actual model. Blog post with samples are coming. 

&nbsp;

There was one other challenge.&nbsp; Windows Azure Mobile Services provides this great wrapper for CRUD operations that allows you to use JavaScript (Node.js) and manipulate requests and responses. It's a great model, but it's not mature and the documentation is not at the level it needs to be at yet.&nbsp; This wasn't a problem with WAMS or Node, just a challenge in early adoption.&nbsp; A big shout out to [Tommy Lee](https://twitter.com/TommyLee) and [Jonathan Rozenblit](https://twitter.com/jrozenblit) of Microsoft for helping throughout the day to get me info, docs and articles.

## The Android Client

The screenshots above are from the WP7 app that we considered to be the "game master". It was capable of reading information from the Regina API and then – through authenticated REST calls to Windows Azure Mobile Services – set new goals for the players.

As for the Android shots below, I have no claim to these...the Android client was put together by Johannes and he was kind enough to send along the screenies. I'm certain that if there was an award for "best Android app" we would have won that as well.&nbsp; Here they are! From left to right, you have the check in point where you reach the goal, the confirmation of your points, the tie in of activities to "check out" and finally the leaderboard.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/image_8.png)&nbsp;[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/image_4.png)&nbsp;[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/image_2.png)&nbsp;[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/image_thumb_2.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/ee3ae11418f7_A726/image_6.png)

A few more days and I should be able to catch up to Johannes!

## World Peace and Other Take-Aways

Folks, a German boy and a Ukrainian boy worked side by side to build an Android game (with a Windows Phone admin console) running on Windows Azure.&nbsp; There is Shalom in Regina.

I was surprised by the number of APIs that are out there, freely available. I was equally jarred by the diversity of apps that teams concocted at the event given that we were mostly working from the same APIs.&nbsp; And this is really why these events shine; the exposure to other people's creative process is quite valuable.

This was a great experience that I am going to do again. If you haven't done a hack event before – and chances are you haven't – I really recommend breaking down some barriers (or excuses) and just go out and do it. You get to work with interesting people and if you want to you get to explore some new tech at the same time.

Have a look at [HackDays.ca](http://hackdays.ca) or search for [local hackathons](http://www.bing.com/search?q=local+hackathons) to find an event near you.