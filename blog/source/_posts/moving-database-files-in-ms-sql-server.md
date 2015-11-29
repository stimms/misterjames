title: Moving Database Files in MS SQL Server
tags:
  - SQL Server
id: 1251
categories:
  - Develop Meta
date: 2011-04-21 20:00:00
---

I was running into a disk space problem that was only revealed when I tried to restore a production database to the test environment that was several GBs bigger.

I updated the DB’s files to point to a new drive on the server, moved the files and restarted the service.&nbsp; All good.

Then, as I attempted to restore the database, I kept getting files pace errors.

The problem? I had forgot to alter the restoration file location and SQL Management Studio was trying to use the original file path.

Here’s where you make the change:

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/Moving-Database-Files-in-MS-SQL-Server_1474D/image_3.png "image")

When you go to Tasks –&gt; Restore –&gt; Database, select the Options page and change the target paths for your database files.