title: Adaptive Performance is Evil
id: 1051
categories:
  - Develop Meta
date: 2011-07-10 18:30:00
tags:
---

I was working on tribute video for [good friends of mine](http://oldblog.jameschambers.com/blog/it-rsquo-s-been-a-long-long-long-weekend) this last week using Adobe Premiere Pro CS4.&nbsp; I have a brand-spanky-new laptop with some pretty rockin’ specs including an nVidia video card that chews through the video processing, for the most part, in real time and as fast as 5 times better.

But I couldn’t get that far into editing. Every minute to 90 seconds Premiere was crashing.

I was getting OpenGL errors, saying “A problem occurred when processing OpenGL commands”, or “Encoding Failed. Could not read from the source” or “The NVIDIA OpenGL driver detected a problem with the display driver and is unable to continue. The application must close”.&nbsp; There was also one about “lost the connection to the device” and the error numbers from Adobe Premiere were error 4 and error 8.

The culprit? Adaptive Performance. It’s built into the GPU drive and by default turned on. It’s supposed to use less power and save the environment or something.

Turns out I hate planet Earth.

**TURN ADAPTIVE PERFORMANCE OFF**. At least for Premiere Pro.&nbsp; If you go into the nVidia control panel you can actually adjust driver settings on a per-application basis (or globally set the feature’s status across the whole OS). Adaptive Performance is a feature that steps down the processing capabilities of your video card to conserve energy and then tries to crank the juice up when you need it.

Unfortunately this causes Adobe Premiere Pro to wig out.

So, turn it off and live happier.
 > A quick note: I found the solution to this on a gamer site when I was Binging the errors and I’m certain it was related to an OpenGL error. I would like to give attribution here but I don’t remember the name of the gaming community and can’t re-Bing the same results I was getting when I found the fix.