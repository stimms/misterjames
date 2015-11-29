title: PowerShell Script Examples for NuGet Packages
tags:
  - CodeProject
  - NuGet
  - Visual Studio 2010
id: 1161
categories:
  - Code Dive
  - Projects
date: 2011-06-09 18:44:00
---

Here is a list of PowerShell examples taken right from the packages you might already be using, some have more detailed explanation or examples elsewhere on the webs. This list intends to be a quick reference for common operations when building install scripts for your NuGet packages.&nbsp; Keep the list handy for writing install for your own packages.

##### _**2011.06.21 Updates**_

### **Import Modules (Using External Scripts for Commands)**&nbsp;
<pre>&nbsp;&nbsp;&nbsp; Import-Module (Join-Path $toolsPath commands.psm1)</pre>

Going forward, the [recommended convention](http://stackoverflow.com/questions/5406406/do-helper-functions-in-a-nuget-packages-init-ps1-have-to-be-global) (from David Fowler) for extending the package manager console is to put your scripts into seperate files (modules) in your tools directory, and to import them with a command similar to above in your init.ps1 file.

### <span style="font-weight: bold">Insert Text into Current Document at Cursor</span>

_from [jQuery.Farbtastic](nuget.org/List/Packages/jQuery.Farbtastic)
_<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $dte.ActiveDocument.Selection.Insert($scriptTemplate)</span>

Use this to write out text to the current cursor location in the active document. A little caution with this one: You might want to check the lenth of the selection first (write-host $dte<span style="color: teal">.</span>ActiveDocument<span style="color: teal">.</span>Selection<span style="color: teal">.</span>Text<span style="color: teal">.</span>Length) becuase this will overwrite any selected text.&nbsp; 

### **Copy Text to the Clipboard**

_from _[_jQuery.Farbtastic_](nuget.org/List/Packages/jQuery.Farbtastic)
<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp; $mystring | clip</span>

In this snippet all we're doing is 'piping' the string into the clipboard.&nbsp; Be mindful that this would overwrite anything already there.

There are lots of interesting ways you can add functionality to your package and the package manager console.&nbsp; I found, in exploring useful ways to get code to the user, that there may be scenarios where just pushing generated code to the clipboard may not be a bad option.

### **Discover IDE Manipulation and Automation Commands**
<pre style="font-family: consolas; background: white; color: black">    $dte <span style="color: teal">|</span> Get-Member
</pre>

This ouputs the list of exposed properties and methods in the DTE object.&nbsp; This is super handy for fiddling with the IDE from the Package Manager Console, even inside a test project, to figure out what is available.&nbsp; Besides, self discovery is more interesting than [reading the docs](http://msdn.microsoft.com/en-us/library/1xt0ezx9.aspx) ;)&nbsp; You can also try plugging around with the properties exposed, such as $dte<span style="color: teal">.</span>Solution <span style="color: teal">|</span> Get-Member, for example.

* * *

### <span style="font-weight: bold">Read Properties from Visual Studio Objects</span>

_[from SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
_<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp; $currentPostBuildCmd = $project.Properties.Item("PostBuildEvent").Value</span>

This gets the post build command from the specified project item.

&nbsp;

### <span style="font-weight: bold">Loop Through All Installed Packages</span>

[_from NuGetPackageUpdater_](http://nuget.org/List/Packages/NuGetPackageUpdater/1.0.0.0)
<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp; foreach ($package in get-package) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; write-host "Package $($package.Id) is installed"
&nbsp;&nbsp;&nbsp; }</span>

This loops through all packages that are installed on the currently targetted project.

&nbsp;

### <span style="font-weight: bold">Loop Through All Projects in the Solution</span>

[_from NuGetPackageUpdater_](http://nuget.org/List/Packages/NuGetPackageUpdater/1.0.0.0)
<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp; foreach ($proj in get-project -all) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; write-host "Project $($proj.Name) is in this solution."
&nbsp;&nbsp;&nbsp; }</span>

This outputs the name of each project in the solution in the package manager console. You can use get-project to retrieve the name of the currently targetted project.

&nbsp;

### <span style="font-weight: bold">Delete an Item in a Project</span>

[_from MvcScaffolding_](http://nuget.org/List/Packages/MvcScaffolding)
<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp; Get-ProjectItem "foo.txt" | %{ $_.Delete() }
</span><span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;</span>or
<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;</span><span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp; Get-ProjectItem "InstallationDummyFile.txt" -Project $projectName | %{ $_.Delete() }</span>

This gets the specified file from the current project and 'pipes' it into the next command, which is a call to delete.&nbsp; The "$_" is a token representing the passed-in object.&nbsp; If you have the name of the project and want to target that project you can also use the -Project argument as in the second example.

_Note: Get-ProjectItem was available when I tried this out because MvcScaffolding has a dependency on T4Scaffolding, which exposes a PowerShell cmdlet of the same name.&nbsp; I will post on this later to show you how to get this to work in your own packages, but if you intend to use scaffolding anyways there's no reason your package can't depend on T4 for now._

&nbsp;

### **Remove a Reference from a Project**

_from [Machine.Specifications](http://nuget.org/List/Packages/Machine.Specifications)_
<pre>&nbsp;&nbsp;&nbsp; $project.Object.References | Where-Object { $_.Name -eq 'NameOfReference' } | ForEach-Object { $_.Remove() }</pre>

If you're looking to purge references that you know are safe to do so when you're uninstalling, you can do so as above. This uses a filter to selectively remove a reference from the project in question.&nbsp; You could also use a foreach at the project level and remove the reference from each project (rather than using the $project param that NuGet feeds you when uninstall is executed).

Alternatively, rather than assuming your user hasn't taken on the dependency in another fashion in their project, you could simply use Write-Host and let them know that if they aren't using the references you've added (during your install) that it is now safe to remove them from the project in question.

&nbsp;

### **Loop Through All Project Items**

_from [MisterJames](http://oldblog.jameschambers.com)_
<pre>&nbsp;&nbsp;&nbsp; ForEach ($item in $project.ProjectItems) { Write-Host $item.Name } </pre>

This will print out the list of all items in the specified project. You can use $item for project item operations per the VS automation docs (see link at bottom), here, I've just output the name.

&nbsp;

### <span style="font-weight: bold">Open a File from the Tools Folder</span>

[_from EfCodeFirst_](http://nuget.org/List/Packages/EFCodeFirst)_&nbsp;_(note that this is a legacy package no longer used)

<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp; $dte.ItemOperations.OpenFile($toolsPath + '\EFCodeFirstReadMe.txt')</span>

This one is pretty straightforward.&nbsp; $toolsPath is passed into our script and is available as a parameter throughout.&nbsp; Here, we're simply calling $dte.ItemOperations.OpenFile to open the file specified.

&nbsp;

### <span style="font-weight: bold">Opening a File from the Content Folder</span>

_[from Glimpse.Mvc3](http://nuget.org/List/Packages/Glimpse.Mvc3)_
<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp; $path = [System.IO.Path]
&nbsp;&nbsp;&nbsp; $readmefile = $path::Combine($path::GetDirectoryName($project.FileName), "App_Readme\glimpse.mvc3.readme.txt")
&nbsp;&nbsp;&nbsp; $DTE.ExecuteCommand("File.OpenFile", $readmefile)</span>

In this example there are two variables at play, an System.IO.Path reference&nbsp; and a string being built to open a file.&nbsp; In the final line, $DTE.ExecuteCommand is called to invoke Visual Studio's "open file" command in a different way from EFCodeFirst, with the filename passed in as a parameter.&nbsp; Note that you could also use [System.IO.Path]::Combine directly; $path is just a reference/alias to access the static method in a more readible manner.

&nbsp;

&nbsp;

### <span style="font-weight: bold">Calling Other Scripts in the Current Scope</span>

**&nbsp;**_[from SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
_<span style="font-family: 'Lucida Console'" face="Lucida Console">&nbsp;&nbsp;&nbsp; . (Join-Path $toolsPath "GetSqlCEPostBuildCmd.ps1")</span>

PowerShell will execute the specified file in the context of the current script when you do a "dot-space-brackets".&nbsp; In this example, we need to concatenate the name of our script, which is located in the package tools directory, to the tools path and then make the call.&nbsp; At this point, any vars you set up will be available to you in the current script.&nbsp; If you want to break out and build a list of files or - in this case - set up a variable with post-build information, this is your vehicle.

&nbsp;

Exploring On Your Own

If you want to accomplish something that you don’t see here but have seen in another package it’s pretty easy to see what they’ve done.&nbsp; You need to get yourself a copy of the [NuGet Package Explorer](http://nuget.codeplex.com/releases/view/59864) – also a great tool for building your own packages – and just start pulling down the work that’s out there.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/2c0f6af1e34b_927A/image_3.png "image")

The best part about using this for exploratory purposes is that it can connect directly to a repository feed.&nbsp; Just pull up File –&gt; Open from Feed… to search or browse packages. The NuGet.org package repository load by default, but you can specify your own.

![image](http://oldblog.jameschambers.com/Media/Default/Windows-Live-Writer/2c0f6af1e34b_927A/image_6.png "image")

### References

You can dive much deeper, of course, as you have PowerShell in your hands here, controlling the IDE.&nbsp; If you want more information on specifics of any of the ideas at play here, check out the following links:

*   [NuGet PowerShell Reference on docs.NuGet.org](http://docs.nuget.org/docs/reference/package-manager-console-powershell-reference)<li>[Visual Studio DTE Reference on MSDN](http://msdn.microsoft.com/en-us/library/envdte.dte.aspx)<li>[Windows PowerShell Reference on TechNet](http://technet.microsoft.com/en-us/library/ee221100.aspx)

&nbsp;

### Wrapping Up

Know any other good samples?&nbsp; A more straight-forward way to pull something off? Feel free to send me a note so I can add them here, or send questions my way.

[CodeProject](http://anyurl.com)