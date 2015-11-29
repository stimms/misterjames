title: Live Connect Developer Center - Application Name Collisions
tags:
  - Azure
  - PrairieDevCon
id: 571
categories:
  - Code Dive
  - Conferences
date: 2012-10-01 17:37:00
---

## No Really, It Works!

![image](http://jameschambers.azurewebsites.net/wp-content/uploads/2012/12/image1.png "image")Today I had the pleasure of presenting on Windows Azure Mobile Services at the Prairie IT and Developer Conference (known as Prairie Dev Con) here in Regina. It was great to see so many people out with such diverse backgrounds. We had people who develop on Android, Windows Phone, iOS and lots of .Net folks who are interested in building apps for Windows 8.&nbsp; 

I had a blast presenting…and getting an app up and running with a cloud-backend for the services side of things with a fully functional Windows 8 client is pretty great.&nbsp; But I ran into one major snafu when I tried to get Push Notifications running and, subsequently, Windows Account authentication.

## What Went Wrong

[![image](http://jameschambers.azurewebsites.net/wp-content/uploads/2012/12/image2.png "image")](http://www.windowsazure.com/en-ca/develop/mobile/)Preparing for the conference I had built out the demo app several times as I dove into WAMS and examined what was going on at each level.&nbsp; Each time I would reset the environments by deleting the application and database and removing it from Live Connect Developer Center.&nbsp; Things were fairly clean.

When I started the presentation things were going fine until I realized that I had forgot to reset after my last run through the night before.&nbsp; When I logged into the management portal, I saw the app was still there, so I deleted the app (and database) and carried on with my demo.&nbsp; 

**Unfortunately**, when I got to enabling push notifications, the Live Connect Developer Center let me go about creating an app with the same name, and it did so without warnings or errors. Though I didn’t have time to solve it during the session (what a great group of attendees though, giving me some space to explore!), what I nailed down was this:&nbsp; when I created a new app – with the same name – I think very simply that I ended up confusing Live Connect as to which app was which.

Further to that, authentication wouldn’t work either. Even though I discovered and deleted the second (well, I guess first) rogue app in Live Connect and verified that my app secret and package ID was correct, I still wasn’t able to authenticate. In the log files for the app in the WAMS portal, I was simple getting a message similar to “invalid client” without further explanation.

I was able to finish out the presentation and give them a sense of how things would have worked, and from the feedback in talking with some devs afterwards, it seems they were fairly forgiving ![Smile](http://jameschambers.azurewebsites.net/wp-content/uploads/2012/12/wlEmoticon-smile.png)

## And So?

**Do not create a second app in Live Connect Developer Center with the same name as an existing app**.&nbsp; This is bad and will break your stride.

I got back to my hotel room, deleted the leftover pieces from the demonstration and started again from scratch – this time giving a unique name to my app – and everything worked from end-to-end without hassle or hesitation – push notifications, Windows Account authentication and the whole like.&nbsp; **It took me 8 minutes.**

There really should be appropriate error messages here so that you can make sense of what’s going on. Or, better yet, the Live Connect Developer Center should slap your wrist and tell you that there is already an app on your account with the same name.

But this one is easy to avoid. Just don’t duplicate your app names!

If you want to see for yourself how easy it is to get started, head over to the [WAMS site](http://www.windowsazure.com/en-ca/develop/mobile/) and get started!

Thanks again to everyone who came out and please remember to fill in those evaluations!