title: Microsoft Virtual Academy – A Lap Around Azure Websites - All the Questions and Links From the Talk
tags:
  - Azure
  - MVA
id: 5311
categories:
  - Beyond Code
  - Conferences
date: 2015-01-14 19:51:01
---

On January 14th, 2015 Jon Galloway and Cory Fowler presented a live “Lap Around Azure Websites” on Microsoft Virtual Academy. I was the online “Community Expert” for the live event and fielded questions in the chat. 

## The Links Shared from the Session

Some questions were more easily answered just by sharing a link! Here are the links that I mentioned in the chat or that were cited by Jon and Cory:

*   Free trial of [Azure Websites](http://try.azurewebsites.net?prid=ca_DevMVP_JC)  <li>An article on [extending AD to the cloud](http://blogs.technet.com/b/keithmayer/archive/2013/01/20/step-by-step-extending-on-premise-active-directory-to-the-cloud-with-windows-azure-31-days-of-servers-in-the-cloud-part-20-of-31.aspx?prid=ca_DevMVP_JC)  <li>Download of the [Azure SDK](http://azure.microsoft.com/blog/2014/11/12/announcing-azure-sdk-2-5-for-net-and-visual-studio-2015-preview/?prid=ca_DevMVP_JC) (multi-VS versions available)  <li>Azure Websites [authentication using Active Directory](http://azure.microsoft.com/blog/2014/11/13/azure-websites-authentication-authorization?prid=ca_DevMVP_JC)  <li>Scott Guthrie's [eBook](http://weblogs.asp.net/scottgu/free-ebook-building-cloud-apps-with-microsoft-azure?prid=ca_DevMVP_JC) on cloud development  <li>The Azure [PowerShell reference](http://azure.microsoft.com/en-us/documentation/articles/install-configure-powershell/?prid=ca_DevMVP_JC) documentation  <li>Activating your [MSDN Subscriber Benefits](http://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits/?prid=ca_DevMVP_JC) for Azure  <li>Configuring on-prem TFS for [Azure deployment](http://www.codewrecks.com/blog/index.php/2013/07/05/deploying-on-azure-web-sites-from-on-premise-tfs/)  <li>Kudu and [customizing your deployment script](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) for Azure Websites  <li>Node.js [tooling](http://nodejstools.codeplex.com/)  <li>Cory Fowler’s most excellent Azure Websites [cheatsheet](http://microsoftazurewebsitescheatsheet.info)  <li>Use [GitHub for Windows](https://windows.github.com/)  <li>Leveraging webhooks with [Kudu](https://github.com/projectkudu/kudu/wiki/Web-hooks)  <li>Configuring [deployment slots](http://azure.microsoft.com/en-us/documentation/articles/web-sites-staged-publishing/?prid=ca_DevMVP_JC#AboutConfiguration) in Azure Websites  <li>Host Azure Websites on-prem with [Azure Pack](http://www.microsoft.com/en-ca/server-cloud/products/windows-azure-pack/?prid=ca_DevMVP_JC)  <li>Testing in Production [documentation](http://azure.microsoft.com/en-gb/documentation/articles/web-sites-staged-publishing/?prid=ca_DevMVP_JC)  <li>[SignalR](https://github.com/SignalR/SignalR) powers the Kudu in-browser console  <li>Great resources on [Azure Web Jobs](http://azure.microsoft.com/en-us/documentation/articles/websites-webjobs-resources/?prid=ca_DevMVP_JC)  <li>[Manage scale](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale?prid=ca_DevMVP_JC) on your website  <li>[Website backups](http://azure.microsoft.com/en-us/documentation/articles/web-sites-backup/?prid=ca_DevMVP_JC) in Azure  <li>[Authentication for ASP.NET apps](http://www.asp.net/mvc/overview/security?prid=ca_DevMVP_JC) on Azure Websites  <li>Hybrid connection [walkthrough](http://azure.microsoft.com/en-us/documentation/articles/web-sites-hybrid-connection-connect-on-premises-sql-server/?prid=ca_DevMVP_JC) for connecting to on-prem DBs  <li>The [Cloud Cover](http://channel9.msdn.com/Shows/Cloud+Cover?prid=ca_DevMVP_JC) show on Channel 9 

As well, here are some other really great MVA sessions you should take advantage of:

*   Building Apps with Node.js Jump Start ([Course](http://www.microsoftvirtualacademy.com/training-courses/building-apps-with-node-js-jump-start?prid=ca_DevMVP_JC))  <li>Introduction to Creating Websites Using Python and Flask ([Course](http://www.microsoftvirtualacademy.com/training-courses/introduction-to-creating-websites-using-python-and-flask?prid=ca_DevMVP_JC))  <li>What’s New in Visual Studio 2013 Jump Start ([Course](http://www.microsoftvirtualacademy.com/training-courses/what-s-new-in-visual-studio-2013-jump-start?prid=ca_DevMVP_JC)) <li>Microsoft Azure Fundamentals ([Course](http://www.microsoftvirtualacademy.com/training-courses/microsoft-azure-fundamentals?prid=ca_DevMVP_JC)) 

And, if you’re into the whole reading thing, I [published a book on Azure Websites](http://amzn.to/1bnErSs) that is still really relevant, in spite of all the new features and changes the portal’s seen.

## The Common and Predominant Questions

I grabbed the questions that stood out the most from the chat and am posting them below, along with my answers.

**_Can Javascript run on .NET platform also?_**
No, JavaScript doesn't run natively on .NET. But Azure != .NET, and Azure Web Sites has many options for web stacks, including Node.js. Remember that JS has traditionally been a client-side language, so it of course can be used in any web application created with .NET

**_Do we need SDK's and command line tools with Visual Studio 2015 Preview._**
No, you don't need it. But the SDK adds some extra (really useful) tooling, and the command line tools gives you options from a console or PowerShell script. 

**_What are your thoughts on using WebMatrix? How does it compare to VS in terms of workflows you are going to talk about?_**
WebMatrix is a great tool for working with non-.Net stacks and has language support beyond what you'll find in VS. However, if you're working with .NET langauges (as well as many other non-.NET languages), VS gives you probably the IDE experience, hands-down. 

**_When signing up for the free trial on Azure Websites it still asks me for entering my credit card details on the trial registration page. Is it going to charge me?_**
Even if they do collect the CC info, there will no charges. By default there is a spending limit of $0 on your account. Unless you remove that, there will not be any charges incurred. You can sign up with confidence!

**_How can you deploy to two Azure websites? One for Staging and another one for Production._**
The easiest way is to use two different branches in source control, then you can automate everything about deployment to two entirely different websites. You can use slots and swapping to help with this.

**_My trial period has expired and I have converted my subscription to Pay-As-You-Go subscription. When will I have to pay something?_**
You'll have to pay if you want to start adding "real-world" value to your project. Things like scaling, dedicated domain names, load balancing and the like.

**_Are MySQL databases hosted on Azure the same as SQL server? What's the deal with these 3rd party partners? How is it different?_**
The third-party DB providers are from partners that may or may not use Azure resources. They provide an integration mechanism with Azure, but don't necessarily spin up Azure resources. With MySQL, you just need a connection string to get at the data. It is not hosted on the same server as Azure SQL. And in the case of ClearDB, they are actually hosted on an Azure cluster.

**_Should a team have their GIT repository in VS online or Azure for an Azure site?
_**There is no preference. In fact, I have on-prem git repos, VS Online hosted and even repos (the majority) hosted on GitHub. You are free to choose what works with your team.

**_Should I add my packages to .gitignore when working with GitHub and Azure Websites? Does Kudu install the packages for me? If so, wouldn't it be faster to deploy by NOT adding the packages to .gitignore?_**
It’s possible that you might save time, but deploying to different regions may mean greater latency as you deploy. Local cached packages can be quickly retrieved by Kudu, which could result in time saving for you.  <p>**_Can I use the Kudu webhooks with a local githook from my Git repo?&nbsp; 
_**The webhook endpoint is exposed in the API, you can read more about it [here](https://github.com/projectkudu/kudu/wiki/Web-hooks).

**_How do deployment slots work? Can I use separate databases for different slots?_**
Slots can have their own configuration, so if you need different connection strings for the alternate slots, [you can do that](http://azure.microsoft.com/en-us/documentation/articles/web-sites-staged-publishing/?prid=ca_DevMVP_JC#AboutConfiguration). This is also true of app settings and the like.

**_What does Kudu do? _**
Kudu is the deployment engine that powers Azure Websites. It's grown into a great set of services that allow you to manage and customize your deployment process. There is a public API you can use in your own projects, integrate with other web hooks and more.

**_How does the Kudu Console work in the Browser?
_**The cool bits to me (as a developer) is the continuous connection to the browser from the server. That's all powered by SignalR, which is another open source project started by some MS folks. You can find the [SignalR GitHub repo here](https://github.com/SignalR/SignalR).

**_What are the costs related to turning on tracing?_**
There is a very small percentage of a performance hit, but nothing really in terms of monetary costs, it’s included in the platform. The one potential thing to watch for is that there is a cap on website space and the logfiles will count towards that.

**_How do I analyze the data from my error logs on my site?_**
You can use the built-in support site from the scm sub-domain on your site. Just navigate to &lt;yoursite&gt;.scm.azurewebsites.net/support and select your hostname. From there, you can drill in and see error and troubleshooting information.

**_Can I use .NET 3.5 on Azure Websites?_**
Yes, this is an option from the configuration menu. The only trouble I’ve had in moving to AW is when you have legacy third-party components that need access to unsafe memory or the registry (not supported on websites).