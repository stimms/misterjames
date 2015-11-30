title: Getting Microsoft Band Running Outside of the United States
tags:
  - Microsoft Band
id: 5961
categories:
  - Develop Meta
date: 2015-04-06 12:26:00
---

If you are lucky enough to get your hands on a Microsoft Band before they’re readily available here in Canada or elsewhere worldwide in unsupported markets, you’ll likely run into a couple problems trying to get started. 

When we tried to get my wife’s Band up and running we ran into a few small but totally surpassable hurdles. Our roadblocks stemmed from being both out-of-market and using a Windows Phone.

For the short version of the checklist to get thing running, skip to the end. For the walkthrough, keep reading.

## Lighting up the Band on Your Phone

[![wp_ss_20150406_0001](https://jcblogimages.blob.core.windows.net/img/2015/04/wp_ss_20150406_0001_thumb.png "wp_ss_20150406_0001")](https://jcblogimages.blob.core.windows.net/img/2015/04/wp_ss_20150406_0001.png)To start using the Band, you need an app on your phone, but it won’t appear in the Store. To get around this, you’ll need to change your Country/Region to “United States”, or another supported market (the app is available now in GB).

With the region set, you’ll be able to install and run the app, just reboot your phone and look for Microsoft Health in the Store. Unfortunately, you’ll hit another snag right away after you install it. Namely, you’re going to hit this error message when you try to launch Microsoft Health (the Band Sync app): _Network Error. Something went wrong. Please check your network connection and try again._

[![image](https://jcblogimages.blob.core.windows.net/img/2015/04/image_thumb.png "image")](https://jcblogimages.blob.core.windows.net/img/2015/04/image.png)

Now, you can start troubleshooting, there are three reasons why you’ll get this error message than I have learned:

1.  You actually don’t have network connectivity. Make sure you’re connected to WiFi or have a good data connection with your cell provider.  <li>[![wp_ss_20150406_0002](https://jcblogimages.blob.core.windows.net/img/2015/04/wp_ss_20150406_0002_thumb.png "wp_ss_20150406_0002")](https://jcblogimages.blob.core.windows.net/img/2015/04/wp_ss_20150406_0002.png)Your username has a special character in it. Likely due to a primarily English release in a limited market, it seems as though the app wasn’t thoroughly tested with letters outside of the English language. Go to the [Microsoft Account site](https://account.live.com), sign in, navigate to “Basic Info”, then check your display name and remove any special characters.  <li>Your phone has a primary language other than English (United States). To change this, head back into settings and ensure that you have English (United States) in the list, and make sure that it is the _first_ one in the list. I have it configured as such (see the image to the right) along with English (Canada) and it works just fine. 

So, these are the solutions for Windows Phone, but I don’t have any iDevices or Adroid phones to test this out with. I wouldn’t be surprised if there were similar issues there as well, so hopefully these tips might be able to help you out.

## The Short List

So, in summary, here’s what you’ll need to get going outside of a supported market with Microsoft Band:

*   A phone with the Country/Region set to “United States” (or another supported market)  <li>A working and active WiFi or data connection  <li>A display name in your Microsoft Account that doesn’t have special characters  <li>A primary language of “English (United States)” (or another supported language) 

## A Quick Disclaimer

Changing the country, region and language settings on your phone may have other unintended side affects. Though it’s easy to change back, you should be aware of this in case other apps start behaving irrationally.

Hope this helps someone out there. A quick thanks to the gentleman on Microsoft Support chat who was completely willing to help troubleshoot, even though we weren’t in the US (he was the one who tipped us off about special characters).

Do you have a Band outside of the US? Do you have an iPhone or Adroid, and have you had any difficulties in getting it sync’d?

Off to keep building my Band app…happy coding everyone! ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/04/wlEmoticon-smile1.png)
