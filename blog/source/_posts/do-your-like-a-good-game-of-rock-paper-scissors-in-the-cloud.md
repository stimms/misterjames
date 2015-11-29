title: 'Do Your Like a Good Game of Rock, Paper, Scissors? In the Cloud?'
tags:
  - Azure
  - Rock Paper Scissors
id: 1081
categories:
  - Code Dive
  - Projects
date: 2011-06-29 18:34:00
---

My head is in the clouds.&nbsp; I just played the most intense game of Rock, Paper, Scissors ever.&nbsp; It was a “best of 1,999” format.&nbsp; And my arm didn’t even get tired.&nbsp; In fact, I played about 40 rounds of it, with 8 different competitors and by the end I was winning _every_ match.

Folks, what I’m talking about is the [Rock, Paper, Azure](http://www.rockpaperazure.com/) competition that Microsoft is hosting right now, and if you are in the least bit interested in the cloud this is a relaxed, fun way to get yourself started.&nbsp; Plus: [free t-shirts](http://www.rockpaperazure.com/aboutchallenge.aspx)!&nbsp; Learning cloud development and a free t-shirt? How can you not?!

### <font style="font-weight: bold">What It Is</font>

The game is hosted on the Azure platform and every participant has a chance to create an entry bot – the “player” used by the game engine to play out&nbsp; your game.&nbsp; You play every other registered player.&nbsp; Each game is played to 1,000 wins, and the first to 1,000 gets 3 points to their name.&nbsp; There are leaderboards, prizes and at the end of the practice rounds a Grand Tournament for the ultimate bragging rights.

### <font style="font-weight: bold">What You’ll Really Learn</font>

Now, this game isn’t so much about cloud development as it is to walk developers through the process of _deployment_.&nbsp; The “bot” project – used to create your player in the game – is set up for you as part of the download.&nbsp; All you have to do is write a bit of c#, vb or f# to determine what move your bot will play next, and you’re off the races.&nbsp; 

There is one catch: your bot can only be entered into the competition if it’s hosted in Azure.&nbsp; Said another way, you **_have_** to deploy to the cloud to enter and win.&nbsp; To get your Azure account (free trial) you can visit the [getting started page](http://www.rockpaperazure.com/getstarted.aspx) for the contest.

### <font style="font-weight: bold">Having Some Fun</font>

Brace yourself, because you may not know this: **RPS is _serious_ game**.&nbsp; Seriously.&nbsp; There are international organizations, widely accepted rules and derivatives, annual competitions with big prize money and the works.

In light of all this seriousness, you can still have fun with this competition.&nbsp; You just have to remember that a “random” throw isn’t going to get you anywhere.&nbsp; In my testing, even a bot that favors a particular throw does better than a random bot.

### <font style="font-weight: bold">(Really) Getting Started</font>

Once you get through the guide and get yourself running, you’re going to want to get some bots in your local Azure emulator so that you can test strategy.&nbsp; If you’re only working in one language, simply remove the other projects from your solution (I work in c#).

Other bot developers aren’t going to give you their bots, but you can easily create some pseudo-strategies to make sure you don’t fail in the simplest of cases.&nbsp; Point in case: an early version of a bot I was making won every time against every bot in my lab, except the bot that through mostly scissors.

So, let’s get started.

1.  Open the BotLab solution with Visual Studio running as Administrator and run the app.&nbsp; While the Azure fabric is getting going, we’ll create some bots to unleash fury in the lab.  <li>![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_25.png "image")Open the MyBot solution in a new VS instance (you don’t need to be admin here). Build the app by pressing Shift+Ctrl+B. Right-click on the project folder and click “Open Folder in Windows Explorer”.&nbsp; Here, along with your bin, obj, and properties folders, create a “bots” folder.  <li>Now, back in Visual Studio, ![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_24.png "image")open the Properties of your project and navigate to the Build Events tab.&nbsp; We need to add a new post-build event similar to “copy $(TargetPath) $(ProjectDir)\bots\”.&nbsp; This will copy our built bot to a bot folder every time we build one.&nbsp; We’re using this as a technique to build a quick army of bots. Every time we build, our target dir gets cleared, so we want to have a bit of a locker to keep them penned in and out of harm’s way.  <li>Next, go back to the Application tab inside your project properties.&nbsp; Change the Assembly name to “ScissorBot”.&nbsp; When you build your app again (Shift+Ctrl+B) your ScissorBot dll will be copied to the bots folder. 

Now we’re ready to get some bots together.

### <font style="font-weight: bold">Building Some Bots</font>

Go to the MyBot class and you’ll see that we inherit from IRockPaperScissorsBot, which ultimately inherits from IBot.&nbsp; We have but one responsibility to fill with this contract: MakeMove.

We want to create some bots that will just favor certain moves.&nbsp; This isn’t likely a strategy that another bot maker will employ, but if by chance they do favor a move, you’ll know that you can fare fairly well against them.&nbsp; Wow. I don’t think I’ve ever used “fare” and “fairly” side be each before.

Replace the meat of your MakeMove function with the following code:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_7.png "image")

This little guy has an affection for sharp things and will throw scissors better than half the time.&nbsp; Build your app and make sure the DLL is in your bots directory.

Now do the following:

1.  Change the Assembly name in your project settings to PaperBot.  <li>Modify the above code above to favor paper.  <li>Build your app and make sure PaperBot’s in that bots folder too.  <li>Do steps 1-3 over for RockBot.&nbsp; Queue epic guitar solo. 

&nbsp;

Now you have some competition…they’re not great, but they’ll give you a fighting chance if your bot can survive against them.

### <font style="font-weight: bold">Testing out the BotLab</font>

Now we get to see how things play out. Pop back over the browser that is running the BotLab. You’ll see a window there with a few sample bots in inventory.&nbsp; Use the form there to select and add the bots you’ve just created. It should look something like this when you’re done:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_23.png "image")

Next, click the “Start Battle!” button and let the rain fall. All bots in the lab will square off against each other and the results will be tabulated for you.&nbsp; By the way, note how far down the list the Random bot is.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_22.png "image")

Clicking on a bot brings up the game log in a little modal dialog that lets you see how things transpired.&nbsp; In this way, you can review the details of the way you played and how you won (or lost) as well (or as badly) as you did. This is how I came to discover that when one of my bots was getting into trouble, it panicked and started throwing water balloons. 

I do want to point out an interesting thing here that is part of the game rules and critical to winning:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_16.png "image")

Notice that after BigBang has gone up 10-0 there are two draws (ties) and then PaperBot wins?&nbsp; The points accumulate through a tie, so PaperBot was awarded three points when Scissors cut Paper.&nbsp; Which brings us to some strategy.

### <font style="font-weight: bold">Some Strategy</font>

What if you knew that the points were accumulating?&nbsp; Well you can. Your class, one instantiated, is stateful for the duration of the game. You can have member fields that hold values and therefore can track some things to gain an advantage.&nbsp; Using this tip from the RockPaperAzure blog, we are able to build a Dynamite-favoring bot that simply OWNs the competition.&nbsp; Rename your assembly and modify your bot’s class to the following:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_7.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_18.png)

Now we’re combining strategies: if we have a tie on the line worth three or more points we’re going to throw Dynamite. If not, we’re favoring a throw (paper).&nbsp; Now when we check our results we see the following:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_21.png "image")  <p>DynaBot wins every time I run this.&nbsp; The one mark in the loss column is _always_ against ScissorBot. 

Time for some smarts!

### <font style="font-weight: bold">Making the Bot a Little Smarter</font>

So, here’s the deal: I want to have a sporting chance in this competition, but I want to throw out some ideas as to approaches you can take to help you win too.&nbsp; I’ll give you the ideas (and some code) and you work out the details.&nbsp; I’ll help you with the code once the competition is done ![Smile](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/wlEmoticon-smile_2.png).

The first thing I did was start to keep track of the last 20 moves of the other player. This is easily done by adding a Queue&lt;Move&gt; to our class.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_9.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_27.png)

Next, I moved my _pointsOnTheLine logic to an UpdateStats method.&nbsp; That’s also where I’ll keep track of the _lastPlays Queue.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_10.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_29.png)

Now I have the ability to do a bit of analysis on the opponent’s moves.

Next, I wanted an easy flag to see if I was currently winning in the game.&nbsp; This lets me switch between offensive and defensive strategies. I started by creating a variable that allows me to just check a bool anywhere else in my code.&nbsp; This is a class-level field.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_11.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_31.png)

Then, in my UpdateStats method, I do the check:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_12.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_33.png)

Finally, I wanted a way to set up a playbook.&nbsp; I created a series of List&lt;Move&gt; collections that easily let me pick my next few moves.&nbsp; Some examples of that are as follows:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_13.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_35.png)

To use these moves, I simply created a class-level queue…

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_14.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_37.png)

…and test to see if it’s empty. If it is, I can load up a few plays rather easily…

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_16.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_41.png)

Get it?&nbsp; If I’m “WINNING”, I foreach over “the Avalance” and add them to the queue.&nbsp; Finally, I can pop my results back off the queue easily and make plays from my populated playbook:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_17.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_43.png)

The last thing I did was to create a simple function that returns the throw that beats whatever you pass in.

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_20.png)

Part of the stats I keep watching for a mirroring strategy (where the other bot just plays whatever I just played).&nbsp; Now I can easily throw back at the mirror and guarantee a win:

[![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_thumb_19.png "image")](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/669fa1007230_9E53/image_47.png)

Now, obviously I’m not giving away all my secrets here, but you can use strategies similar to this to get the ball rolling.&nbsp; I wrote three _really good bots_ with different strategies…in a list of 20 bots I created, one of these three always won.&nbsp; Now, I’m working on the bot that can beat all the bots in my arsenal.

### <font style="font-weight: bold">Next Steps</font>

All that’s left to do is deploy your bot to the cloud. Head over to the game site and [watch the video](http://www.rockpaperazure.com/getstarted.aspx#4) on deployment; that is, after all the point of this game: to see how easy it is to deploy a project to the cloud.&nbsp; When you’ve finished deploying, you can then enter the contest and start kicking bot butt!

Good luck!