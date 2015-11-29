title: Scheduled Jobs in Windows Azure Web Sites
tags:
  - Azure Websites
id: 2551
categories:
  - Uncategorized
date: 2014-02-15 23:24:45
---

Last year I published a pretty good primer on Windows Azure Web Sites (available on Amazon as [Windows Azure Web Sites](http://www.amazon.ca/gp/product/B00E5BI5L6/ref=as_li_qf_sp_asin_tl?ie=UTF8&amp;camp=15121&amp;creative=330641&amp;creativeASIN=B00E5BI5L6&amp;linkCode=as2&amp;tag=chasthelist-20)![](http://ir-ca.amazon-adsystem.com/e/ir?t=chasthelist-20&amp;l=as2&amp;o=15&amp;a=B00E5BI5L6) ) but the Azure team keeps coming out with new features. This post will walk you through the creation of scheduled jobs on Windows Azure Web Sites.

## Tasks That Aren’t Part of Your Web Site’s UI

On demand reports are a great feature but don’t give your users as-at reporting. Artifacts from operations on your site can use up disk space. Sometimes, you’d prefer to have a digest of information sent out, rather than a notification on every interesting event.

If you need to run some kind of process on a regular interval a solution might be this handy feature on Azure Web Sites: scheduled jobs. These types of requirements can sometimes be met with BI tools, but might cover any kind of activity which may or may not be associated with data, such as:

*   a nightly report  <li>a cleanup script  <li>sending an email  <li>pushing an SMS message hourly to your phone for new account signups 

I have a console app for the purpose of this article that I’ve created that runs some reports.&nbsp; You’ll see later similar output as the logs from the cloud-run copy of this application.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image8.png "image")

Yes, my reports start at 0\. Don’t judge, my brothers and sisters.

I’m using an EXE here, but you can use any of the following:

*   Windows CMD (.cmd, .bat or .exe)  <li>Node.js (.js)  <li>PowerShell (.ps1)  <li>PHP Scripts (.php)  <li>Python (.py), or  <li>Bash (.sh) 

Your script or executable needs to be in a ZIP file which can also include any other files you need for processing such as configuration, images to embed in email messages, etc.

## Configuring the Job

Click on the Web Jobs tab in the web site’s dashboard, then click “Add” from the command bar at the bottom of the site or from the dashboard page that loads up.

[![image](http://jameschambers.com/wp-content/uploads/2014/02/image_thumb1.png "image")](http://jameschambers.com/wp-content/uploads/2014/02/image9.png)

If you haven’t already signed up for the Schedule preview program, you’ll not yet be able to create scheduled jobs, but’s it’s trivial to setup and the link is provided on the Web Job screen.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image10.png "image")

Follow the link and complete the sign up; it’s a straightforward button click.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image11.png "image")

With that in place you can continue with creating the job. I wrap up all my needed files into a zip, and then I pick my options on the first page of the job setup:

![image](http://jameschambers.com/wp-content/uploads/2014/02/image12.png "image")

Finally, I create my schedule and configure it to run every day for a year:

![image](http://jameschambers.com/wp-content/uploads/2014/02/image13.png "image")

Your job is added to the list and then runs on schedule, or you can run it on demand from the command bar. After executing, logs are added to your account:

![image](http://jameschambers.com/wp-content/uploads/2014/02/image14.png "image")

Clicking the link brings you to the history of job runs where any console output is available for viewing. Error logs, should any rise, are also saved out here. 

![image](http://jameschambers.com/wp-content/uploads/2014/02/image15.png "image")

You can see the output by drilling into the log, which is unsurprisingly similar to what we saw from our console&nbsp; output at the start of this article.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image16.png "image")

## Understanding Job Storage Requirements

The job ZIP that you create can be up to 200MB and will be stored in your web site’s corresponding file system. Logs are also saved out, albeit in a slightly different path. 

Job scripts are saved at: D:\home\site\wwwroot\App_Data\jobs\triggered\JOB_NAME

Job logs are saved out at: D:\home\data\jobs\triggered\JOB_NAME

This is actually really great info to know, because with your job script saved in your application’s App_Data directory, you have the ability to manipulate the configuration files (if any) for your script.

Keep in mind that the storage needs for your jobs are factored into your web site’s storage restrictions, so jobs that generate output need to be monitored to make sure you’re not exceeding your quota.

## Getting at the Raw Files

There is a great – and growing – administrative back door called Kudu to your Windows Azure Web Site that you may not be aware of. It helps with all kinds of things like SCM checkin hooks, deployment tasks, or viewing logs. You can reach it at this location:
 > http://your_site_name.scm.azurewebsites.net 

It’s basically the URL that you use to access the host on azurewebsites.net, but you plug in the scm.&nbsp; There is a debug console that gives you the ability to plug away through your files in the Kudu menu.

![image](http://jameschambers.com/wp-content/uploads/2014/02/image17.png "image")

## Wrapping Up &amp; Next Steps

Windows Azure Web Sites now easily allows you to create and manage jobs that can be executed on demand or on a schedule. You ZIP up your files, feed them into the site and then configure the execution times for each of your scripts through the dashboard for your site.

Now go solve some scheduled job need, and happy coding!