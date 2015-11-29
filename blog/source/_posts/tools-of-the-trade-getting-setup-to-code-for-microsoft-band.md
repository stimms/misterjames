title: 'Tools of the Trade: Getting Setup to Code for Microsoft Band'
tags:
  - Azure
  - Microsoft Band
id: 6181
categories:
  - Code Dive
date: 2015-04-22 07:25:00
---

There is a very competent and capable app for your Microsoft Band in your App Store called Microsoft Health, which is available for iOS, Android and, of course, Windows Phone. But Microsoft Health is just one way you can expose and relay information from the myriad of sensors you’ve got on your wrist, and you’ve likely got ideas on where you’d like to take it.

In my [first article](http://jameschambers.com/2015/03/iot-for-humans-and-developers-getting-started-with-my-microsoft-band/) I wrote briefly about IoT in general, and how a device like Microsoft’s Band really opens the door for .NET developers looking to take part in the wearables game. In this article, I’m going to lay out the tools and frameworks that you’ll need in order to develop for the Band.  We’ll also start up a project and install the components we need to get our app ready to talk to the device. If you’re a fairly experience developer and want to dive right in, jump down to the end for the summary and have at’er.

## Setting up Your Cloud Account

As we collect data from the sensors, we’re going to want to feed it up to the cloud so that as we develop the backend of the app, we have access to all the data.

It only takes about 3 minutes and 17 seconds to give yourself a free shot at using cloud services. Seriously. So [pop over to Azure](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119) and get yourself signed up for the free trial.
> [Signup for Azure Here](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119)
Also, check out [this link for information on MSDN](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575136) as you may also qualify for free monthly Azure benefits as well.

## Readying Your Development Environment

Next, we need a bit of tooling. This isn’t a complicated list and you likely have everything you need already, however, I speak to, meet with and mentor many developers who are locked into older versions of tooling, so here’s a list to follow along with so that we’re all on the same page:

*   Microsoft Visual Studio 2013 ([MSDN](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575136) or [Community Edition](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx))
*   USB cable for your Windows Phone
*   USB charging/sync cable for your Band (included in the box)

I have a plethora of USB cables from a variety of sources. I have noticed that the Nokia cable that came in the box with the charger is a better quality cable and have had less problems with it than others (from cheap chargers, other phones and cameras).

## Creating the Project

This part’s pretty easy, just pop open Visual Studio, select File –&gt; New Project, and then switch to the Store Apps/Windows Phone Apps category. From there, Pick the Blank App template and name your project **AliveAndSafe**.

[![image](http://jameschambers.com/wp-content/uploads/2015/04/image_thumb2.png "image")](http://jameschambers.com/wp-content/uploads/2015/04/image2.png)

We’ll talk more about the application we’re creating next time, but if you’ve read my [previous post](http://jameschambers.com/2015/03/iot-for-humans-and-developers-getting-started-with-my-microsoft-band/) you know where I’m going with this. ![Winking smile](http://jameschambers.com/wp-content/uploads/2015/04/wlEmoticon-winkingsmile.png)

## Adding the Band SDK

Next up is getting the components in our project that we’ll use to interact with the Band and trap information relayed to us from the device itself.

Open up the Package Manager Console (View –&gt; Other Windows –&gt; Package Manager Console) and type the following command:
<pre class="csharpcode">install-package microsoft.band -pre</pre>
As expected, this pulls down the DLLs needed for our app and adds references to:

*   Microsoft.Band
*   Microsoft.Band.Phone
*   Microsoft.Band.Store
Having a peek inside, we can see a number of interesting objects that we can create, collections to iterate over, sensors to subscribe to and states that we can inspect.

[![image](http://jameschambers.com/wp-content/uploads/2015/04/image_thumb3.png "image")](http://jameschambers.com/wp-content/uploads/2015/04/image3.png)

We’re going to dive into these in more detail as we build out our app.

## Designing for Microsoft Band

An important thing to note is that the Band has a really strong and capable design language. After wearing it for only a couple of hours I felt like I had a good sense of what it was trying to do and where I needed to go, which is amazing given the number of sensors and the limited UI that the device presents!

As such, we should likely be good citizens here and respect the design intent that the device put forth. You can find more information on what the designers were thinking by downloading the [Microsoft Band Visual Guidelines](http://developer.microsoftband.com/docs/MicrosoftBandVisualGuidelines.pdf) document.

## Next Steps

Okay…easy lifting today, here’s what we did:

*   Prepared our development environment by grabbing [Visual Studio](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575136), signing up for [Microsoft Azure](http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200575119), and getting our cables.
*   Created a Windows Phone application using the Blank App template.
*   Added the Microsoft.Band pre-release package from Nuget to pull in our dependencies.
In the next article we’re going to talk a little bit more about our application that we’re building and get a tile onto the Band to represent our application.

Happy Coding! ![Smile](http://jameschambers.com/wp-content/uploads/2015/04/wlEmoticon-smile3.png)