title: Setting the Resolution Using the Camera API in Windows Phone 7.1 Mango
tags:
  - Windows Phone
id: 981
categories:
  - Code Dive
date: 2011-08-09 18:22:00
---

I'm working on an app that, believe it or not in today's ultra-high-res-world, doesn't require much resolution at all. The images are used as part of a simple on-screen sequence, and while they may ultimately be saved to local storage, they'll only ever be displayed in a 100x100 box on the display.

Changing the resolution is fairly easy: we simply pick one of the resolutions available on the device and assign it to our PhotoCamera instance.&nbsp; Note that you can't pick a random resolution and set it on your own whim, it has to be a resolution supported by the hardware/config of the actual handset.

You can find code on MSDN similar to the following that walks through the PhotoCamera.AvailableResolutions to discover what is available on the user's device:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Setting-the-Resolution-Using-the.1-Mango_12E48/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Setting-the-Resolution-Using-the.1-Mango_12E48/image_2.png)

In my case, however, I'm not really interested in iterating through the list; I want the smallest resolution possible. LINQ to the rescue!

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Setting-the-Resolution-Using-the.1-Mango_12E48/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Setting-the-Resolution-Using-the.1-Mango_12E48/image_4.png)

Likewise, if you want to use the highest available resolution, modify the query to use OrderByDescending instead of the straight OrderBy.&nbsp; You can, of course, make that ultra-terse by making a direct assignment.

You must first initialize the camera instance before you set the resolution.&nbsp; That means two things: you're going to have to assign the camera instance to the source of a VideoBrush and you're going to have to write an event handler for your Initialized event on the camera. As you spin up the form, you're find yourself doing something like this:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Setting-the-Resolution-Using-the.1-Mango_12E48/image_thumb_4.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Setting-the-Resolution-Using-the.1-Mango_12E48/image_10.png)

...and then in the event handler you're going to want to move back to the UI thread and set the resolution:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Setting-the-Resolution-Using-the.1-Mango_12E48/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Setting-the-Resolution-Using-the.1-Mango_12E48/image_8.png)

And there you have it!

### <font style="font-weight: bold">Side Note of Developer Love: The Windows Phone 7 APIs Make Me Happy Inside</font>

I just want to mention this at this point: the WP7 APIs are awesome.&nbsp; I am helping to mentor a friend of mine as he broadens his skill set and I totally dig the opportunity to teach him how to write good code. We've the weirdest mentorship set up where I, a .NET guy for a decade now, am teaching a guy to program in Java on the Android dev stack.&nbsp; Needless to say, we're both doing a lot of learning.

So here's the deal...unless I'm missing some magic cloud of awesomeness that the Android folks have cleverly buried away, WP7 devs seem to be at a distinct advantage.&nbsp; Case in point: Camera API.&nbsp; Want to display a live preview on WP7?&nbsp; Create a camera instance, assign it to a brush.&nbsp; Done. Android?&nbsp; Get out the fairy dust and sacrifice a unicorn.&nbsp; Sure there's a ton of samples out there, but good luck finding one that follows the same kind of convention that any other sample uses, and better luck still you manage to get it to run as-is on your version of the dev tools.

At any rate, my buddy's plugging through the Android stuff and I'm having a blast helping him through the learning curves of everything from properties and constructors through to web service access and hardware APIs.&nbsp; I'm developing the app in the WP7 world and loving the experience, and he's drooling over Blend and slugging it out in Eclipse.&nbsp; He's come along way since "what's a class?" and it's only been a couple of months...today we were even having a conversation about threading!&nbsp; I'm looking forward to having another person in these parts whose face doesn't glaze over when I mention "protocol" or "inheritance".