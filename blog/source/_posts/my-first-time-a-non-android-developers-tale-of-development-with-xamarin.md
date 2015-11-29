title: 'My First Time: A Non-Android Developer’s Tale of Development with Xamarin'
tags:
  - Android
  - Visual Studio 2013
id: 2411
categories:
  - Code Dive
  - Develop Meta
date: 2014-02-04 03:51:02
---

Even though I largely sit on the Microsoft technology stack, it would be without reason to leave development on other stacks unexplored. The old adage – jack of all trades is a master of none – used to plague me as a younger developer as I tried to get my hands into _everything_ and found it hard to become a master of _anything_.&nbsp; So, though I’ve kept abreast of what my development brethren on iOS and Android have been up to (and taking much notice of their market share compared to my platform of choice) I have only dabbled to insignificant measure with either.&nbsp; 

I would like to give a shout to my buddies Mike and Brad who have entertained me at length with conversations and code comparisons on both iOS and Android, respectively, as I work on Windows Phone.

But there’s a cross-over class now – highly functional, feature-rich and, better still, it’s “native” to the development experience I know and love in Visual Studio.

My previous comparison was quite jagged; the Visual Studio Express SKU for Windows Phone is free and installs with a double click. “Hello World” is literally seconds away, post-installation when you’re cutting a Windows Phone app. But, when I last tried Android development with Eclipse, there were several downloads, patches, a video card update (yes, seriously, for my L502X) and numerous animal sacrifices required to get the development environment and emulator running.&nbsp; And I really like my cat, so that didn’t go so well.

Enter into the mix Xamarin’s solution to building apps, with a twist that .Net developers are going to love.

### I’m Going to Need a Few Things

[![image](http://jameschambers.com/wp-content/uploads/2014/02/image_thumb.png "image")](http://jameschambers.com/wp-content/uploads/2014/02/image.png)From the get-go, the Xamarin install experience is smart and well-informed. People still make bad installers in 2014, but I can’t accuse Xamarin of that. Like any good citizen, this one knows what it needs to know to get your PC up-and-running. A quick inventory to avoid downloading the parts you already have, then it’s off to cyberspace to fetch the bits. Grab a coffee.

After pulling about 1.5GB down (thank goodness for fast interwebs) the installer runs without much prompting and preps your box with the goods.

Compared to my last experience? So far, this is aces, baby. Each of the installed target platforms even pops up web pages corresponding to the latest version in the Xamarin Developer Center. No errors, only confirmations. Seamless install.

I open up Visual Studio and from my File –&gt; New Project experience I get this:

![image](http://jameschambers.com/wp-content/uploads/2014/02/image1.png "image")

Creating the project gives me a prompt for my Xamarin credentials, which then activates my subscription.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image2.png "image")

Visual Studio is well equipped to give me the lay of the land through the Solution Explorer. You can see the project layout, look at files that make up the solution and even drill into classes to get at the method level-of-detail. I see some interesting bits and drill in.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image3.png "image")

I do the most natural thing in the world to any dev familiar with Visual Studio and hit F5\. I want to see what this baby does. I get the comically honest message:

![image](http://jameschambers.com/wp-content/uploads/2014/02/image4.png "image")
 > You are about to launch the MonoForAndroid_API_10 emulator. Google Android emulators are slow. Do you wish to proceed? 

Yes. Yes, I do. **But!!** First I need to make sure that I’m using the correct emulator. In my case I had selected an Ice Cream Sandwich project template, so I needed to update my emulator selection to the MonoForAndroid_API_15 option. On my little 2 core i7 with 8GB RAM, the first-start for the virtual device and deployment took about 8 minutes, so, that previous message about taking a little time to get things going is pretty true. That said, the first run also needs to fire up the emulator, push the SDK out, then install the app and sync the assemblies. Seconds later, I have a working app. Hello World!

![image](http://jameschambers.com/wp-content/uploads/2014/02/image5.png "image")

## Bells and Whistles. Because Awesome.

I return to the IDE, press the Stop control for the debugger and dig into the code. I set a breakpoint on an interesting line of code and re-run the app.&nbsp; Are you kidding me? Sweet! I’m debugging an Android application in Visual Studio.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image6.png "image")

That interesting line of code allowed me to assume something given the project structure I had previously seen, so I drilled into the folder called “Resources” where you wouldn’t be too surprised to find a “Layout” folder, followed by a “Main.axml”. Double-clicking this file gave me a well-equipped toolbox and a rich designer with draw and source modes and a convenient device selection for preview purposes.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image7.png "image")

## Wrapping Up

“Guess what, Mom, I’m an Android developer!” That right there, that is **not **on the top of the list of phone calls I am going to make in 2014\. There’s obviously lots more to familiarize myself with, but this establishes a coherent base: I have a great development experience from a trusted company (Xamarin and Microsoft [announced partnership details](http://blog.xamarin.com/microsoft-and-xamarin-partner-globally/)) that is [winning awards](https://xamarin.com/visual-studio) for the work they do, in the best integrated development environment PERIOD working with a language I love.

In the months ahead I’m going to be talking a _lot_ more about Reactive Applications, and one of my goals is to make sure that I’m providing examples for cross-platform experiences. I’m working closely with my good friend [Simon Timms](https://twitter.com/stimms) to explore concepts related to RA on the Microsoft stack in the _back end_, but these applications are designed for scale and the reality is that most of your potential client base may exist on a different platform.

Sure, it’s easy to be nervous when you do it for the first time, but then you realize you were likely making a bigger deal out of it than necessary. When you’re well-equipped, there’s really no reason to feel any kind of anxiety over experimentation. Oh, and for the record, I’m still talking about Android development.

## Next Steps

I’ll be writing soon on my other adventures, particularly with building out cloud-based solutions. These will really, _really_ scale well to serve as the platform for client apps on all kinds of platforms, Android included. If you want to get in on the mix of things, be sure to prep yourself with the following:

1.  Hit the [Xamarin web site](http://xamarin.com/) and sign up for your trial. #WorthIt.
2.  Get familiar with your target: Android design specs are [readily available](http://developer.android.com/design/get-started/principles.html).
3.  Check out the excellent starter community on [Xamarin’s site](http://docs.xamarin.com/).&nbsp; Docs, to recipes, to tutorials, and all in the context you choose – xplat or platform-specific.