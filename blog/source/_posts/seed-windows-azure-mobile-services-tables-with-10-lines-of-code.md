title: Seed Windows Azure Mobile Services Tables with Less Than 10 Lines of Code
tags:
  - Windows Azure
  - GenFu
id: 1461
categories:
  - Code Dive
date: 2012-12-28 18:22:00
---


# Seed Windows Azure Mobile Services Tables with Less Than 10 Lines of Code

Looking for an easy way to build realistic test or sample data out for your app?&nbsp; You only need two packages and a few lines of code to seed whatever kind of table you need.

I won't lie, when you want to throw a few dozen records into a table, this is pretty sexy:

    var peopleTable = _client.GetTable<person>();
    var people = Angie.FastList<person>();
    foreach (var person in people)
    {
        peopleTable.Insert(person);
    }

## How to Make it Happen

1. Create a Windows Azure Mobile Services instance and add the tables you need. In my example above, I'm using a table called "Person".&nbsp; There is only one field created to start with called "Id".&nbsp; Leave this as-is because we'll use Dynamic Schema (on by default) to flesh out the table.
2. Create a new Console app in Visual Studio 2012.
3. Install two packages from the Package Manager Console:
        1. Install-Package Heyns.ZumoClient&nbsp; – [ZumoClient][1] is a use-anywhere .Net 4.0 library that wraps Windows Azure Mobile Services, provided by [Deon Heyns][2].
        2. Install-Package AngelaSmith – [AngelaSmith][3] is the object initializing smarty-pants that can populate your properties with realistic-looking data, created by [Dave Paquette][4] and [James Chambers][5] (that's me!).

With those things in place, you simply need to instantiate your client, table and a list of Person objects.&nbsp; Finally, iterate over your collection and push it to the cloud.&nbsp; That looks like this:

    _client= new MobileServicesClient("https://yourapp.azure-mobile.net/", "yourkey");

    var peopleTable = _client.GetTable<person>();
    var people = Angie.FastList<person>();
    foreach (var person in people)
    {
        peopleTable.Insert(person);
    }

And you're set!&nbsp; Run that bad boy, wait a couple of minutes or so, then check your Mobile Services table in the Windows Azure portal.

![image][6]

## Going Further

You may have more complex data to set up, or you may even have requirements on how the properties need to be filled. For this, you can use the fluent methods on Angie to tell her how you want the data created.

Here's how you would get 100 users worth of scores in a leaderboard on Windows Azure Mobile Services:

    var entryTable = _client.GetTable<leaderboardentry>();

    var entries = Angie.Configure()
        .ListCount(100)
        .FillBy("UserName", delegate() { return string.Format("{0}{1}", Susan.FillFirstName(), Susan.FillLastName()); })
        .FillBy("LastUpdated", delegate() { return Susan.FillDate(DateRules.Within1Year); })
        .MakeList<leaderboardentry>();

    foreach (var entry in entries)
    {
        entryTable.Insert(entry);
    }

## Next Steps

Okay, time class participation. These are early versions of these libraries but you can help make them more awesome.&nbsp; Pop over to GitHub and fork and contribute!

I've also written [a primer on using AngelaSmith][7] here on my blog.

Good luck!

[1]: https://github.com/DeonHeyns/Heyns.ZumoClient
[2]: https://twitter.com/DeonHeyns
[3]: https://github.com/MisterJames/AngelaSmith
[4]: https://twitter.com/Dave_Paquette
[5]: https://twitter.com/canadianjames
[6]: https://jcblogimages.blob.core.windows.net/img/2012/12/image5.png "image"
[7]: http://jameschambers.com/2012/12/introducing-angelasmith-the-object-initializing-smartypants/
  </leaderboardentry></leaderboardentry></person></person></person></person>