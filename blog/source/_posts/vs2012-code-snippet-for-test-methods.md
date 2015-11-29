title: VS2012 Code Snippet for Test Methods
tags:
  - Unit Testing
id: 1691
categories:
  - Code Dive
  - Develop Meta
date: 2013-02-07 18:14:42
---

I don’t post about unit testing, but when I do, lather, rinse, repeat.

Here’s a snippet to generate a AAA-friendly test method that can be invoked by typing _aaa_ and then pressing tab. Better if you do it in a test class #JustSaying.&nbsp; Tested in VS2012, works here ![Smile](http://jameschambers.com/wp-content/uploads/2013/02/wlEmoticon-smile.png). The great thing about VS2012 – and to be honest I’m not sure what version this awesome was introduced – you can edit code snippets on the fly in Visual Studio and test them as you save them, without closing the file or having to restart Visual Studio.&nbsp; Brilliant. I have been doing the VS restart for years, and just tripped over this feature. 

Oh, right, the code snippet!
<script src="https://gist.github.com/MisterJames/4732853.js"></script> 

([View Gist on GitHub](https://gist.github.com/MisterJames/4732853))

This snippet is based on Roy Osherove’s recommended [test naming convention/theories](http://osherove.com/blog/2005/4/3/naming-standards-for-unit-tests.html). There’s no general consensus on what constitutes the “correct” test name, but this is pretty sound as far as conventions go.