title: "Got Windows 7? Here's an Easy Way to Dual-Boot Windows 8"
tags:
  - Visual Studio 11
  - Windows 8
id: 941
categories:
  - Develop Meta
date: 2011-09-28 18:17:00
---

If you want to run Windows 8 in a virtual machine or boot from a VHD there are already instructions out there for that.&nbsp; What about dual-booting on an existing machine?&nbsp; Yeah. That’s why you’re here.

I have a good sized HDD and lots of free space on my lap top.&nbsp; I wanted to run Windows 8 on the metal but didn’t want to format (and lose my perfectly good Win7 config) or swap HDDs every time I wanted to switch OSes.&nbsp; I also wanted to try out the Windows 8 graphical boot loader, so here’s what I came up with.

## The Process

Here’s what you need:

1.  Download Windows 8 from [http://dev.windows.com](http://dev.windows.com)  <li>Either burn the ISO or [create a bootable USB](http://maketecheasier.com/create-windows-8-usb-boot-disk/2011/09/21) (I like &amp; used the USB option)  <li>About 40Gb of free space on your drive  <li>Maybe up to 2 hours of passive time (waiting on drive operations) 

The steps are pretty simple:

1.  Go to computer management and shrink your drive  <li>Reboot your computer with the DVD/USB plugged in 

And that’s it!

## Here’s What It Looks Like

First we’re going to need to open up the Disk Management snap-in. It’s located in Computer Management under **Storage**.&nbsp; You access it by right-clicking on **Computer** in the start menu and selecting **Manage**.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_2.png)

If your drive is older or you’re a little crunched for space, right-click your target drive and pull up the properties.&nbsp; You can defrag your drive and possibly shuffle a little more room out of your disk.&nbsp; Note this is completely optional because it looks like shrinking might try to do a lighter version of defrag when it runs.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_thumb_2.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_6.png)

Next, right-click the drive and select **Shrink Volume**.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_4.png)

Windows happily goes off and does it’s best to figure out what it can do for you.&nbsp; Go get a coffee, especially if you have a bigger drive.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_8.png)

Once that is out of the way, give yourself the space you need for Windows 8.&nbsp; There is a recommended minimum of 25Gb, but if you want to install the full version of Visual Studio 11 you’re going to need at least another 10Gb.&nbsp; I used 40Gb and I'm finding that’s a good size to explore the OS and do a bit of development.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_thumb_4.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_10.png)

Click **Shrink** and away you go!&nbsp; Prepare for another wait while it does the dirty work for you.&nbsp; Plug your Windows 8 bootable USB/DVD in the drive and restart your computer, and select your new partition as the destination.

After the install, you’ll have dual-boot glory and Windows 8 running on the metal.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_thumb_5.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/e5a9731de44e_743D/image_12.png)

## An Important Tip

If you’re using a USB drive to install (perhaps with DVD, didn’t try) you’re going to want to pull out the key when Windows 8 tries to reboot.&nbsp; Otherwise, it’ll start the process over again. Save yourself 5 minutes of waiting to reboot and pull the USB stick out AFTER your screen goes black and preferably BEFORE your POST finishes.

## What’s Next

1.  Developers, dive in! It’s easy to create new apps using the included Visual Studio 11 Express, or if you have an MSDN subscription, you can grab the full preview of VS11\.  <li>Forget what you know about using Windows and just try to embrace Metro. Play with the apps that they’ve included.&nbsp; Look at how those apps integrate with each other, and figure out where it’s appropriate for your app to expose Share or Search contracts.  <li>Try to rethink an app or part of an app you’ve recently worked on and design it for Metro. It’s a different paradigm and we need to think about things that perhaps weren’t always in our domain as Windows developers (application activation &amp; state, connectivity/network availability, and more importantly style). 

&nbsp;

Good luck with your dual boot!