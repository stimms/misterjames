title: Changing the Namespace With Entity Framework 6.0 Code First Databases
tags:
  - Entity Framework
id: 2621
categories:
  - Code Dive
date: 2014-02-20 03:54:48
---

Sometimes a refactoring of your project includes changing the namespaces used throughout your project. If you’re using Entity Framework 6.0, this type of change can have an impact on EF’s ability to detect the current status of your namespace. This article helps you to mitigate any conflicts and allows your migrations to stand as-are. 

I cover a bit of background, but you can jump to the end to see two or three possible fixes.

Also, sending a thanks out to my good friend [David Paquette](https://twitter.com/Dave_Paquette) for the review on this post.

## My Beef With Existing Fixes

If you change namespaces and run into problems, you might see an error similar to the following:
 > <font face="Lucida Console">An exception of type 'System.Data.SqlClient.SqlException' occurred in EntityFramework.dll but was not handled in user code</font>
> 
> <font face="Lucida Console">Additional information: There is already an object named 'AspNetRoles' in the database.</font> 

I’m not sure I would call this misleading, but it certainly doesn’t explain the problem in the clearest of terms. Here’s what I would prefer to see, particularly in the Additional Information section of the error message:
 > <font face="Lucida Console">Additional information: The namespace for the Entity Framework migration does not have any corresponding migrations in the target database. It is possible that your connection string is not configured correctly, that you are attempting to update a database that was created without migrations enabled, or that your namespace for your configuration class has been modified. Query the migrations table on database *database_name* to review current migration state.</font> 

Sure, it’s verbose, but it lets you in on what might be happening.

Specifically, migrations are tracked by ContextKey in the __MigrationHistory table and the key includes namespace information. When your namespace changes, you also need to update the records in the DB that correspond to migrations that have already been executed.

My beef with existing fixes? Most of the time when you see similar errors come up the answer seems to ignore the fact that people might be using this stuff in production. Namely, the fix tends to be “drop your database and let the Framework rebuild it”, which, sure, I mean, it solves the problem. It’s just not good for business.

## Behind the Scenes

For each change to your database tracked with with a migration, a hash representing your model is computed and stored in order to detect the next set of changes that occur. As you execute the migration, the hash is added, along with the MigrationId and ContextKey to the migrations table.

When you attempt to access your data through the DbContext and you’re using, for example, an initializer such as MigrateDatabaseToLatestVersion, the Framework will attempt to play catch-up and make sure the database reflects the current model in your application. To do this, it queries the database to see where the database thinks it’s at, and it uses both reflection over and information from your configuration and context classes. You can see the queries that run if you capture the chatter with SQL Profiler:

[![image](https://jcblogimages.blob.core.windows.net/img/2014/02/image_thumb2.png "image")](https://jcblogimages.blob.core.windows.net/img/2014/02/image18.png)

And if you drill into the details you’ll see something like the following as the Framework tries to figure out where you’re at:

[![image](https://jcblogimages.blob.core.windows.net/img/2014/02/image_thumb3.png "image")](https://jcblogimages.blob.core.windows.net/img/2014/02/image19.png)

I’ve dashed out my namespace as this was work for a client, but you can see the root of the problem here. The Configuration class is in the <font face="Lucida Console">Root_Namespace.Migrations</font> namespace; if you move the class to a new namespace, this query is modified to reflect it, but previous migrations stored in the database are not.

Your configuration is automatically created for you when you enable migrations; it’s a class that exists in a namespace which is based on the default namespace of your project. <font face="Lucida Console">Root_Namespace.Migrations.Congifuration</font> is also the value that is written to the migrations table as the value for ContextKey. 

That is our vector to correct the problem.

## Building Out The Fix

The first and easiest approach is one that works locally, and could meet your needs if you have access to all affected databases. All you have to do is execute a modified version of the SQL script below:
<pre class="csharpcode"><span class="kwrd">UPDATE</span> [dbo].[__MigrationHistory] 
   <span class="kwrd">SET</span> [ContextKey] = <span class="str">'New_Namespace.Migrations.Configuration'</span>
 <span class="kwrd">WHERE</span> [ContextKey] = <span class="str">'Old_Namespace.Migrations.Configuration'</span></pre>
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

You should be golden at that point, however, this won’t work if you’re doing continuous integration with a team of developers, or if you have a continuous deployment strategy in place. For that, the solution lies in adding the following code to your database Configuration constructor class:
<pre class="csharpcode"><span class="kwrd">public</span> Configuration()
{
    AutomaticMigrationsEnabled = <span class="kwrd">false</span>;
    <span class="kwrd">this</span>.ContextKey = <span class="str">"Old_Namespace.Migrations.Configuration"</span>;
}</pre>
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

This strategy is more durable and will help prevent any of your teammates (or production servers) from running into the same issue. Of course, this keeps your old namespace hanging around in your migrations table, but it does the trick.

A potentially more elegant solution would be to create a database initializer that takes the old and new namespace into account and corrects the migrations table if necessary. This could end up being considerably more work, so you’d have to evaluate if it makes sense for your project and timeline. You can reference an example implementation here: 

[MigrateDatabaseToLatestVersion Source on CodePlex](https://entityframework.codeplex.com/SourceControl/latest#src/EntityFramework/MigrateDatabaseToLatestVersion`.cs "https://entityframework.codeplex.com/SourceControl/latest#src/EntityFramework/MigrateDatabaseToLatestVersion`.cs")

Cheers, and happy coding!