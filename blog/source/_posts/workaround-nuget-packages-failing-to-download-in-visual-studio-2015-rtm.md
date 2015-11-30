title: 'Workaround: NuGet Packages Failing to Download in Visual Studio 2015 RTM'
id: 6791
categories:
  - Uncategorized
date: 2015-07-23 14:59:39
tags:
---

I haven’t figured out a common theme yet, but certain packages are failing to restore when you attempt to install them from the NuGet primary feed via the project.json file in Visual Studio 2015\. Thanks to [Brock Allen](https://twitter.com/BrockLAllen) for confirming I wasn’t going insane.

A couple of things I’ve discovered:

*   This seems to be more common for prerelease packages
*   It seems to work if the package has a previous release version (not in pre)

As a workaround, you can add the packages manually via the dialog in Visual Studio, just make sure you hit that pre-release flag:

![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image3.png "image")

If that doesn’t work for you – sometimes I’m not seeing the package above in my feed – if you have it you can add another NuGet feed to an alternate package source, like I’ve done here with AutoFac’s nightly build feed:

[![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image_thumb1.png "image")](https://jcblogimages.blob.core.windows.net/img/2015/07/image4.png)

The other thing is that once you get it installed in your system cache, it will resolve it from there, which I imagine makes it harder to triage for anyone trying to figure out what’s going on.

I’m seeing various confirmations of this on Twitter:

![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image5.png "image")

![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image6.png "image")

With NuGet 3 being release (and part of VS 2015) I think some package authors are unsure if it’s their problem or what the case may be. Depending on the method you come at it, it’s possible that you can still get the package, but I would say it seems unpredictable right now.