title: (Almost) 8 Web Services in 75 Minutes - With Chad McCallum
tags:
  - PrairieDevCon
  - Regina
  - WCF
  - Web Services
id: 531
categories:
  - Code Dive
  - Conferences
date: 2012-10-03 17:36:00
---

I've been presenting for over a year now and I can honestly say that one of my favourite sessions to have taken part in so far was this past week at the Prairie IT &amp; Developer Conference in Regina.&nbsp; [![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Almost-8-Web-Services-in-75-Minutes---Wi_495B/image_3.png "image")](https://twitter.com/i/#!/CanadianJames/media/slideshow?url=http%3A%2F%2Ftwitpic.com%2Fb05rhn)[Chad McCallum](https://twitter.com/i/#!/CanadianJames/media/slideshow?url=http%3A%2F%2Ftwitpic.com%2Fb05rhn), a regular contributor to the tech scene in Regina, was presenting his web services session, and I was to present a talk on Web API. We realized before the conference that there was going to be a lot of conceptual overlap, so we made the call to merge the sessions and deliver some awesome. ![Smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Almost-8-Web-Services-in-75-Minutes---Wi_495B/wlEmoticon-smile_2.png)

# The Setup

So here's how we did it: Chad's talk normally revolves around him demonstrating how easy it is to build out web services in the .NET world, not only with Microsoft tech, but with libraries available through the open source community as well. Other times he's presented, Chad demonstrated a console app where he consumed whichever service he was building so that the audience could see the service in action.

For our tandem presentation, we decided on a twist: while he was writing the service, I would use a copy of the pre-built service and build a client or a service call through some kind of tooling.&nbsp; The catch? The audience got to pick what I would be using.&nbsp; As he was about to start building his service, the audience picked from a list of possible clients that I would build, and I had anywhere from 4-10 minutes to consume the service.

Here's the client list the audience got to choose from:

*   WinForms  <li>Asp.Net MVC  <li>Console App  <li>Windows Phone App  <li>Browser Request  <li>Fiddler Request 

And the services Chad was building were:

*   Asp.Net ASMX Web Service  <li>Asp.Net MVC 3 (JSON result)  <li>[Asp.Net MVC 4 Web Api](http://www.asp.net/mvc)  <li>[WCF](http://msdn.microsoft.com/en-us/library/bb332338.aspx)  <li>[WCF REST](http://msdn.microsoft.com/en-us/magazine/dd315413.aspx)  <li>[WCF AJAX](http://msdn.microsoft.com/en-us/library/bb412167.aspx)  <li>[Nancy](https://github.com/NancyFx/Nancy)  <li>[ServiceStack](http://www.servicestack.net/) 

I'm intentionally not linking to documentation on the first two because if you are starting a new project, there are very few reasons why you would start there. If you're already building in those spaces, you'll know how to get the resources.

The fun part was that with almost 50 permutations there was no way we were going to build these out ahead of time, so this was all live-in-front-of-the-audience coding.&nbsp; Chad built the services, I built the clients, all in front of almost 50 sharp and scrutinizing devs. It was awesome.

# The Result

For the most part, everything turned out great!&nbsp; We had a couple of glitches as I'll describe below, and we geeked out a little too much in some areas so we only got through 7 of the services, just 1 short of our goal.&nbsp; Here's what didn't go _quite_ as expected:

*   The pre-built services used a simple ECHO method, but during the presentation Chad decided to go all rogue on me and use a .ToUpper in his service method.&nbsp; Even though I finished the client in time – and it worked – the audience picked up real quickly that the text returned and displayed on my client was not converted to upper-case. There was no slight of hand here, just a crossed wire. Full credit to Chad for going further on demonstrating a more interesting method, and even more to the gentleman who picked up on it!  <li>When I built the Windows Phone app and tested quickly before showing the result, I had tested with the word "Regina", and it worked a treat. But when I demo'd the client for the audience, I punched in "Hello, Regina...". Unfortunately, I didn't URL-encode the request, so the punctuation threw off the request/service/response love and it errored out on me. ![Sad smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Almost-8-Web-Services-in-75-Minutes---Wi_495B/wlEmoticon-sadsmile_2.png)&nbsp; I quickly found the mistake and reran the app to show it was working, but it's worth pointing out that even in simple scenarios you'll need to test with more than a simple "Hello" string. 

# What's Next?

Well, Chad and I really didn't dive too deep into the daily practical uses of these, or what scenarios the project types could be used in, but we did give the group a few things to chew on:

*   Are you building something for internal use, or public use?  <li>Do you intend to open up your API?  <li>Do you plan on making mobile apps?  <li>What are your security considerations, and what do users authenticate against?  <li>What have you worked with in the past, and what do you plan on working with down the road? 

Those are five simple questions that will help determine the approach to take for your project. Chad and I are both of the mind that JSON/REST services are probably what you are going to be looking for, but if you can disqualify this approach we presented a myriad of options and alternatives.

# The Most Important Thing

What we really wanted to show is that someone comfortable with programming on the .NET stack can easily try these things out. If you count both our times, Chad building the service and me building the client, we took from 8 to 20 minutes to build _BOTH_ sides of the equation. In less than half an hour you could be exploring any one of these service approaches with pretty much any kind of client.&nbsp; So, go File –&gt; New Project yourself some service love!

# Want a Little More?

*   [Check out our slide deck that we used](http://rtigger.com/blog/2012/03/30/web-services/)  <li>[Check out the service code on GitHub](https://github.com/ChadMcCallum/WebServiceExamples.NET) (the client codes will be posted there too)