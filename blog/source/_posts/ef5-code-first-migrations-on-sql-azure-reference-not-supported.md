title: EF5 Code-First Migrations on SQL Azure - Reference Not Supported
tags:
  - Asp.Net MVC
  - Azure Websites
  - Entity Framework
id: 411
categories:
  - Develop Meta
date: 2012-12-01 17:21:00
---

I am using EF5 Code-First as my database-backing poison. I'm building a web site – deployed to Windows Azure Web Sites – and have been really enjoying the seamless execution of migrations and the easy publish capabilities of Visual Studio 2012 and Windows Azure.

## Some Things Go Bump

I just completed a migration that took a text field and normalized it, creating a separate table and populating that with the distinct values from the original text.&nbsp; The migration pretty much worked itself out, except I needed to write INSERT and UPDATE statements to populate the strings in the new table.&nbsp; This allowed me to then turn around and push the ID of the newly create entity back to the original table.
<pre class="csharpcode">    Sql(<span class="str">"INSERT INTO [Database].[dbo].[DestinationTable] ([Name]) select distinct FieldName from SourceTable"</span>);
    Sql(<span class="str">"UPDATE [Database].[dbo].[SourceTable] SET [NewFKId] = (select PKID from DestinationTable where [Name] = FieldName)"</span>);
</pre>
<style type="text/css">.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }
</style>

As a thought exercise, imagine you were storing your Cities and States as text in a table called Locations.&nbsp; You want to break State out into a separate table, and you want a reference to the State stored in your Location table, not the string version of that state.

With the generated migration, and my code above appended to the end of the Up() method, I was ready to go. Everything worked locally and away I went.

## Then Came Production

Happily coding along, all my tests passing, I completed the related UI changes, committed my work and published to Azure using Web Deploy and saved Publishing Profiles.

Unfortunately, when the deployment completed and the site ran, I got this message as part of a YSOD.

> Reference to database and/or server name in '[Database].[dbo].[DestinationTable]' is not supported in this version of SQL Server

Gulp. Nothing like taking down a live site.

## Side Note: What I Should (and _Would_) be Doing

Since I have multiple projects in the solution and I'm running with various web.config transforms, I'm unable to currently use GitHub publishing. It's still early for this source control support in Azure, so there's green pastures to look forward to.&nbsp; I'm rolling my own CI here, and I have the project on GitHub, so I can't fully automate. Thankfully, I have some PowerShell goodness going on that makes deploying to the cloud easy-peasy-lemon-squeezy and just one command from the console.

Down the road, when multi-site solutions and web.config transforms are supported through GitHub publishing, I'll be able to use Windows Azure Web Sites' built in capabilities for deployments. If something goes south, you just roll back to the last known good version of the site.

## An Easy Fix

While the message points directly at the source of the problem, it's still not clear on what you need to do to resolve it.&nbsp; As it turns out, the limitation is in SQL Azure's ability to use the three-part name of the table.&nbsp; This doesn't really matter, because I'm already connected to the database and I can just drop the name from my statements like so:
<pre class="csharpcode">    Sql(<span class="str">"INSERT INTO [dbo].[DestinationTable] ([Name]) select distinct FieldName from SourceTable"</span>);
    Sql(<span class="str">"UPDATE [dbo].[SourceTable] SET [NewFKId] = (select PKID from DestinationTable where [Name] = FieldName)"</span>);</pre>
<style type="text/css">.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }
</style>

I republished the site and everything worked as I wanted.

So, remember, if you're getting the message that your database or server name reference isn't supported in SQL Azure, and if you're actually connected to the database in question, just drop the reference from the name to curtail this error.&nbsp; If you're getting this error because you are legitimately trying to connect to the other table, you should likely read up on it [over here](http://blogs.msdn.com/b/sqlnativeclient/archive/2010/02/12/using-sql-server-client-apis-with-sql-azure-vversion-1-0.aspx?Redirected=true) for some strategies (one of which might be loading your data into memory on the client to do the work you're looking to do).

Cheers!