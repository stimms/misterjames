title: CustomBinding on Service Reference Results in TransportBindingElement Error
tags:
  - Asp.Net MVC
  - SOAP
  - Visual Studio 2010
id: 921
categories:
  - Code Dive
date: 2011-10-12 18:15:00
---

I’m coding an ASP.NET MVC 3 web site in Visual Studio 2010.&nbsp; I added a service reference to a SOAP 1.2 version of a web service and started updating my code (I was using a SOAP 1.1 endpoint previously).

Unfortunately the switchover wasn’t as clean as I had hoped. The first method I changed resulted in an error, even on a call that appeared identical to the previous version. The error message I received was:
 > _The CustomBinding on the ServiceEndpoint with contract 'SubscriberServices' lacks a TransportBindingElement.&nbsp; Every binding must have at least one binding element that derives from TransportBindingElement_. 

…which looked like this:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/c2efd7958743_EA38/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/c2efd7958743_EA38/image_2.png)

For the quick fix, head to the end of this article.&nbsp; Keep reading if you want to know what’s going on.

### What Was Going On?

The two things that gave me the strongest clues where “CustomBinding” and “TransportBindingElement”, but for different reasons.&nbsp; CustomBinding was a little unexpected as the service was added via HTTP. I guess I always assumed that it would be using an HTTP binding? The other piece suggested a derivative of the TransportBindingElement, which according to [MSDN](http://msdn.microsoft.com/en-us/library/system.servicemodel.channels.transportbindingelement.aspx) is an abstract base class.

### Off to Our Config File

Actually, at this point, I wanted less noise, so I quickly created a console app and added references to both services.&nbsp; The differences were, at this point, fairly obvious. The 1.1 version of the service _was_ using a basicHttpBinding element (which I was expecting), whereas the 1.2 service was using a custom binding:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/c2efd7958743_EA38/image_thumb_1.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/c2efd7958743_EA38/image_4.png)

So, what we’re looking at is trying to get the custom binding to know which transport we want to use. This is something that is implicit in the basicHttpBinding but needs to be explicitly set for customBindings.

### The Fix

Following the links on the TransportBindingElement will lead us to the [httpTransportbindingElement](http://msdn.microsoft.com/en-us/library/system.servicemodel.channels.httptransportbindingelement.aspx), which has both code and config file samples.&nbsp; Adding a short and simple section to the binding did the trick; all I needed was an httpTransport element in the customBinding\binding element. The updated config section looked like this with the MSDN sample config injected:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/c2efd7958743_EA38/image_thumb_3.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/c2efd7958743_EA38/image_8.png)

[Further reading](http://msdn.microsoft.com/en-us/library/ms405872.aspx) shows that all the properties set in the MSDN example are actually all defaults, which means that our httpTransport element can simply be reduced to a single, self-closing tag:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/c2efd7958743_EA38/image_thumb_4.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/c2efd7958743_EA38/image_10.png)

Great!

### Quick Recap

So, in short, if you’re using a SOAP 1.2 service reference, or any service reference that requires the use of a custom binding, make sure you’ve added a TransportBindingElement (such as httpTransport) to your config file or in your code.