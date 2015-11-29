title: 'Day 5: Bootstrap for the Asp.Net Developer'
tags:
  - 30DaysMvcBootstrap
  - Asp.Net MVC
  - MVC
  - MVC5
id: 3131
categories:
  - Code Dive
date: 2014-06-05 10:20:00
---

This is an installment in a 30 day series on Bootstrap and the MVC Framework. To see more, check out [Day 0 for an index](http://jameschambers.com/2014/06/day-0-boothstrapping-mvc-for-the-next-30-days/).

Starting with a blank slate and little direction on where to take things visually is a painful part of web development to overcome, especially for those of us who are in the “design challenged” camp.&nbsp; The [Bootstrap](http://getbootstrap.com/) front-end framework takes away a lot of the guessing and rework that head-ends most new projects and establishes a design language to work within, while still providing options for look-and-feel via themes.

While we’ve had a number of options over the years as the Asp.Net templates have evolved, most of them have stuck out like sore, outdated thumbs and rarely would you let them find their way into production. Today, we have a starting point that marries us to a visual style that we can be happy with publishing.

## Bootstrap is CSS and JavaScript

You’ll include two resources in a page that you want to build off of Bootstrap, the style sheet and the JavaScript library that make it work.

![image](http://jameschambers.com/wp-content/uploads/2014/06/image8.png "image")

The CSS aspect gives you a stock option with fonts, colors and components that work well together, a responsive layout grid, and the flexibility to completely modify the framework’s default colors, spacing and other variables. You can see how diverse things are at the [Bootstrap expo site](http://expo.getbootstrap.com/), and build your own theme variant using the [tools online](http://getbootstrap.com/customize/). You can also download freely available themes from various sites on the interwebs.

The JavaScript library introduces a number of behaviors and widgets that augment the design with a user experience that most web surfers are now familiar with when working with alerts, tool tips, tabs buttons and more.

## Bootstrap is Also Custom HTML Attributes 

The next aspect to be aware of is that the framework relies on several custom HTML attributes to kick up the juice. The JavaScript library looks for these attributes to append functionality, help with layout and behaviors and attach events. 

[![image](http://jameschambers.com/wp-content/uploads/2014/06/image_thumb1.png "image")](http://jameschambers.com/wp-content/uploads/2014/06/image9.png)

The above example, from the docs site, shows how to create a “scroll spy” with no code. Note that all things you can do via attributes can be done with JavaScript as well, so you’re not locked into a model.

What I really like about this approach is that the framework isn’t as opinionated as, say, jQuery UI about how you must go about your business. The attributes are an easy way with minimal code to augment your page.

## Bootstrap is Built In

As of version 5, Microsoft has elected to make Bootstrap the framework of choice in every non-blank web application, making it easy to start working with.

![image](http://jameschambers.com/wp-content/uploads/2014/06/image10.png "image")

It’s included in the bundles, configured at app startup, and it’s included on all child pages that leverage the default layout, located in your Views\Shared folder.

## Bootstrap is Pretty Easy

Once you get your head around the opinions that Bootstrap _does_ have, you’ll find that creating a toggle button styled in the same way as everything else on your site becomes quite trivial.

![image](http://jameschambers.com/wp-content/uploads/2014/06/image12.png "image")

Of course, there is more to Bootstrap than just toggle buttons, and there is more to the MVC Framework than spitting out static HTML, so we have some work to do to finish exploring this dynamic duo.

## Next steps

Tomorrow we’ll look at how we can start to change the way that we render our content, leveraging MVC and the Bootstrap library together.