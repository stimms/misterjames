title: Errors With AdControl in Windows Phone 7.1 (Retail 7.5)
tags:
  - Windows Phone
id: 911
categories:
  - Code Dive
date: 2011-11-07 18:14:00
---

I recently implemented the AdControl in the trial version of an app I've got in the marketplace. It is remarkable how easy they've made it to add ads to your app, with the Microsoft Advertising ASK baked right into the Windows Phone 7.1 SDK.

Unfortunately, I couldn't get the app displaying ads. I assumed there was something wrong with the way that I was implementing it, sent a couple emails out and went about finishing the other features I was working on. The error I was getting was:
 > A first chance exception of type 'Microsoft.Advertising.Mobile.Shared.AdException' occurred in Microsoft.Advertising.Mobile.dll 

No detail, wrapping the control code I was using to add the control to the page with a try block produced no results, so I moved on.&nbsp; Unfortunately, as the ad was not appearing, I forgot that I had stubbed it in and published the update to the marketplace.&nbsp; Oops.

When I saw the update appear in the Marketplace, I installed the app and...there were the ads.&nbsp; In the paid version.&nbsp; Oops.

### Check Your Manifest at the Door

Wait a minute! Everything I've read says that if you can't see ads in the debugger that the ads won't show up on the live app. So what gives?

In looking for help on the error, I hit the SDK documentation where I stumbled on the control's [events](http://msdn.microsoft.com/en-us/library/hh495454(v=MSADS.20).aspx), where I found the ErrorOccurred event.&nbsp; This little beauty solved all my problems.&nbsp; Subscribing to this event – as easy as <font color="#666666" face="Lucida Console">yourAdControl.ErrorOccurred += [tab] [tab]</font> – and checking the properties on the exception provided the details on my problem: I was missing the ID_CAP_IDENTITY_USER declaration in my application manifest.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/203c741d6e15_11BF2/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/203c741d6e15_11BF2/image_2.png)

### So Why Did It Work on the Live App?

Ah, right...when you submit your app to the Marketplace, Microsoft kindly builds your manifest for you with all the correct capabilities.&nbsp; If you have extras, they take them out. If you don't have the ones you need, they inject them.

So, when you accidently publish your app with the AdControl in it, but you're missing the capabilities from your manifest, fear not! The App Hub publishing process will take care of everything.&nbsp; 

Just try to remember to avoid pushing out your app with ads in the paid version.

### Going Forward

So, here's some things you should do when using the AdControl:

*   Familiarize yourself with SDK if you're wondering about monetizing your app.&nbsp; It's super simple.  <li>Make sure you're properly registered with Pub Center and follow the instructions they give.&nbsp; It's straightforward.  <li>Add an event handler to the AdControl.ErrorOccured event. If you run into any issues with the control, it'll set you straight. 

&nbsp;

Good luck!