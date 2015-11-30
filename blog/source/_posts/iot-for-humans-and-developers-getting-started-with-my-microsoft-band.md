title: IoT For Humans (and Developers) - Getting Started with my Microsoft Band
tags:
  - Microsoft Band
  - NuGet
id: 5821
categories:
  - Beyond Code
  - Develop Meta
date: 2015-03-31 12:16:45
---

Making the “Internet of Things” make sense to observers outside the industry is going to take a lot of work. While we might grow frustrated with [the overuse](http://www.networkworld.com/article/2464007/cloud-computing/gartner-internet-of-things-has-reached-hype-peak.html) and below-required understanding of the term, I would argue that in the industry we have a long way to go to fully understand the implications of security, health, privacy, etiquette, social dynamics, productivity and individualism.

But while the term may seem obscure, especially hiding being other abstractions like “connected devices” and mixed in with other buzzwords like “big data”, the reality is that…it’s here.

The auto industry embraced the “of Things” part really well quite some time ago, well before mainstream internet in fact, and has been increasingly good and getting the “Internet” part as well.

As a consumer, it would appear that my van sends me an email when it needs an oil change; in reality there is a query or a push occurring at some set interval or cued off of some trigger when I’ve reached a certain amount of mileage. That push or pull stores a flag in a database, which is later picked up by some service, likely processed through some kind of templating engine and mixed with my past service records and ultimately delivered to my inbox. But, dang, my van sends me emails, Mom!

## Bringing IoT Home For .NET Developers

Even to me as a developer, while I’ve sensed these “Things” emerging around me it’s still been hard to find a way to explore them. You can’t get application development guidance or a NuGet package for your Chevy Uplander.

In many ways our phones have really become these interconnected devices as well, becoming more and more sensor-laden in a world where LTE and Wi-Fi signals have broader reach. Android watches and even the Pebble watch spaces have SDKs if you’re familiar with those development ecosystems or cross-plat tools to target specific devices.

But last week, my Microsoft Band showed up. And there is both an SDK and guidance freely available. As a developer on the .NET stack and solid knowledge of Azure, I can’t help but feel that my playground just got a lot bigger.

## What I Get as a Tinkerer

So, for about $200, and leveraging my Azure benefits, I am like a kid in a candy shop. Here’s what I have:

*   A crazy amount of sensors (GPS, UV, heartrate, accelerometer, gyrometer, ambient light, microphone)
*   “Free” pairing with any major phone platform via the SDK
*   More sensors available via the phone
*   A myriad of connectivity options (Wi-Fi, Bluetooth, NFC)
*   Local and cloud-based storage
*   User accounts, identity services
*   Haptic and audio feedback mechanisms
*   Multiple interaction surfaces
*   Other cloud services (SMS, email, batch processing)
*   A human attached to all of the above
*   The ability to create applications on my laptop/PC/tablet that ties these things together<!--EndFragment-->

You can sign up for Azure for a [free trial here](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119), or look into your benefits through the [MSDN program](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575136), as you may already have monthly credits to burn up.

I haven’t quite figured out what I want to build yet, but there seems to be a lot of interesting paths.:

*   A “Safe and Alive” monitoring application for high-risk occupations (I’m already exploring this)
*   A immersive game with haptic, visual, audio, SMS and touch interactions
*   A reminder app to keep active, with an accountability partner using similar kit
*   An application that leverages the Band for build server (or other service) failures
Remember that some of these become quite interesting when you consider the fact that you don’t have to have your phone out or even in your pocket for these to work, just as long as the Band can maintain it’s Bluetooth connection. You can leave your phone when it gets the best signal (in the vehicle, at the entrance to a storage facility) and still have your app running, collecting, pushing, pulling and processing data.

## So What Now?

These are exciting times, and I’m having fun exploring practical and completely impractical uses and applications. My kids, 13, 11 and 5, are infinite sources of hilarity in discussing what we can do with it. My wife, too, and I’ve found all of a sudden that without talking about “consumer applications”, “big data” and “interconnected devices” I have completely engaged the entire family in the “Internet of Things”. Oh, and I had to order my wife a Band as well. ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/03/wlEmoticon-smile2.png)

I’m going to start sharing my experiences while developing an application in the weeks ahead, delving into the use of the sensors on the Band, exploring interactions and trying to test it in real-world scenarios. Stay tuned to follow my path. My next post will be a “Hello World” app that introduces you to the tooling and SDK.

Have you got a Band? What applications are you thinking about developing? Do you have any successes or failures to share in regards to the “Internet of Things”?