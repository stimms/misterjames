title: When Things Go Wrong During a Presentation
tags:
  - Entity Framework
id: 1141
categories:
  - Conferences
  - Develop Meta
date: 2011-06-13 18:40:00
---

So what do you do when you have a room full of people and your presentation goes awry? Make sure you know your domain before you go in.

I just finished my talk on the spanky, fun Asp.Net Mvc Framework, and I thought I’d prepared well prior to going in.&nbsp; Aside from a few minor snags – also called teaching opportunities ![Winking smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/When-Things-Go-Wrong-During-a-Presentati_EFCD/wlEmoticon-winkingsmile_2.png)&nbsp; – things ran rather smoothly and the initial feedback on the session was quite positive.&nbsp; Running into errors and exceptions is okay, in my mind. We’re programmers after all and 90% of what we do is wrong ;o)&nbsp; I’ve been through most scenarios, especially in this stack, and so I know how to respond.

But then it hit me: a curve ball from the audience (thanks for the questions by the way!) took me a little off course (which I love) and I tried to figure out what the problem was (which, completely topical to my talk, was scary).&nbsp; And, after all, it’s important to know what color scary things are so that you can identify them easier.&nbsp; 

Sad face here: I hit an exception I hadn’t seen before.&nbsp; On stage. In front of 50 awesomely patient folks.&nbsp; ***sigh***

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/When-Things-Go-Wrong-During-a-Presentati_EFCD/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/When-Things-Go-Wrong-During-a-Presentati_EFCD/image_2.png)

That image reads:
 > Model compatibility cannot be checked because the database does not contain model metadata. Ensure that IncludeMetadataConvention has been added to the DbModelBuilder conventions. 

Unfortunately I was a little too nervous to tackle it beyond a couple of quick attempts, but the solution was quite simple: **drop the database and let the EntityFramework recreate it.**

### What Went Wrong

I think the biggest issue was that I was playing with multiple changes to the model classes during the presentation.&nbsp; The metadata associated with the database, stored in its own table where the db is created, got out of sync with what was in the database and what was in the classes in the project.&nbsp; Not a biggy, but frustrating just the same. 

I will note again that this isn’t something you’d likely run into late in a project, only early on. 

Thankfully at the front end of a project we are okay with dropping data, especially when we’re working with something like EF Code First…if we really want data in those tables after we make changes we can easily create our own db initializer and add seed data.

Hopefully, when you run into this kind of error, you won’t be on stage. ;o)&nbsp; At least, not without having first read this post.