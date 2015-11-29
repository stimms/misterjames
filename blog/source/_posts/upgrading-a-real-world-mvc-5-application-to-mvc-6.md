title: Upgrading a Real-World MVC 5 Application to MVC 6
tags:
  - ClearMeasure
  - MVC5
  - MVC6
id: 6752
categories:
  - MVC6 Conversion
date: 2015-07-23 19:19:13
---

These are exciting times for web development on the Microsoft stack, but perhaps a little confusing as well. For many years the cycle of moving from one solution and project system to the next hasn’t been overly complex. Sure, there have been breaking changes, I’ve felt those pains myself, but provided the framework you were using continued to live on, there was a reasonable migration path.

Moving to MVC 6 is going to be a big shift for a lot of development teams, but that doesn’t mean it needs to be scary, complicated or introduce instability into your project.

It does, however, mean that you’re going to need an attitude of learning, that you’ll pick up some new tooling, you’ll have to brush up on your JavaScript and work with some new concepts.

## Let’s Make it Happen

I’m super excited to now be part of the excellent crew at [Clear Measure](http://clear-measure.com/), where this type of attitude seems to be fostered, encouraged and embodied by other members of the team and, more importantly, the management.

We’re now undertaking the process of converting from MVC5 =&gt; MVC6 with our [Bootcamp workshop project](https://github.com/ClearMeasureLabs/ClearMeasureBootcamp) and I have the privilege of blogging my experience with it as I go. [![image](http://jameschambers.com/wp-content/uploads/2015/07/image_thumb2.png "image")](http://jameschambers.com/wp-content/uploads/2015/07/image7.png)We’re going to keep the project building and operable as we go, such that at an point it can be shipped to production or branched for feature development.&nbsp; We’ll be using GitFlow, feature branches, continuous integration and continuous deployment.&nbsp; Our check-ins will be code that builds cleanly with passing tests.

**And,** for those of you who come join in our our MVC Masters Bootcamp sessions, you’ll also get to work on this code base with all the tools, exposure to pair programming, a dedicated product owner and 3 days of intense coding.

[![image](http://jameschambers.com/wp-content/uploads/2015/07/image8.png "image")](http://clear-measure.com/)**Shameless plug**: If you want to level up your team of developers, please reach out to [Gina Hollis](mailto:gina@clear-measure.com??Subject=MVC%20Masters%20Bootcamp) at Clear Measure to plan an on- or off-site event. We promise to melt your minds.

## How We’re Getting There

[![image](http://jameschambers.com/wp-content/uploads/2015/07/image_thumb3.png "image")](http://jameschambers.com/wp-content/uploads/2015/07/image9.png)Well, to start it off, we’re beginning with our initial commit as the MVC 5 project [Jeffrey Palermo’s](https://twitter.com/jeffreypalermo) been using in the Masters Bootcamp for some time.

The application is hosted on [GitHub](https://github.com/ClearMeasureLabs/ClearMeasureBootcamp) and you can [see the issues](https://github.com/ClearMeasureLabs/ClearMeasureBootcamp/issues/) that we’re identifying and working through. We’re doing the whole thing as open source in hopes that other teams can learn from what we learn in the process.

And, as I knock items off the issue list I’ll be posting about them here, covering the challenges, pitfalls and wins we encounter along the way. You can bookmark this post for updates in the project. Feel free to ask questions on the issues in the repository, or ping me on Twitter ([@CanadianJames](https://twitter.com/CanadianJames/)).

Stay tuned!

*   Part 1: [Moving to VS2015 from VS2013](http://jameschambers.com/2015/07/moving-to-vs-2015-from-vs-2013/)
*   Part 2: [Getting Your Build Server Ready for VS2015](http://jameschambers.com/2015/07/getting-your-build-server-ready-for-vs-2015/)
*   Part 3: [Retargeting Projects to .NET 4.6](http://jameschambers.com/2015/07/upgrading-projects-to-net-4-6/)
*   Part 4: [Multitargeting Projects with VS2015 Projects](http://jameschambers.com/2015/08/converting-net-4-6-projects-to-the-vs-2015-project-system/)