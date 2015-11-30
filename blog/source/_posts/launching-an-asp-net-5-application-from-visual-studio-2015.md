title: Launching An ASP.NET 5 Application from Visual Studio 2015
tags:
  - ASP.NET
  - MVC
  - Visual Studio 2015
id: 7331
categories:
  - Code Dive
date: 2015-07-29 14:00:00
---

If you are trying to use any DNX (DotNet Execution) runtime other than dnx451 (i.e. dnx452, dnx46) you will run into the following error when running the application from Visual Studio 2015, when used with the initial release of the Beta 6 tooling:
> **The current runtime target framework is not compatible with 'YourWebApplication'.**
> 
> Current runtime Target Framework: 'DNX,Version=v4.5.1 (dnx451)'
>  Type: CLR
>  Architecture: x64
>  Version: 1.0.0-beta6-12256

If you’re instead running with a debugger attached, you won’t hit a breakpoint, you’ll only get a 500\. It doesn’t matter what framework runtimes you have installed on your machine. It doesn’t matter what your global.json says or what dependencies or frameworks you take or specify in project.json.<p>This is because the default runtime for launching IIS Express from Visual Studio is indeed dnx451\. You can get around this in one of two ways:

1.  Launch the website from the command line in your project directory using the command “dnx . web”. Web is a command that is exposed in your project.json and shares the needed info (config) to launch a project-specific instance of IIS.
2.  In your project properties (right-click, properties from Solution Explorer), add the following environment variable in the Debug tab:
&nbsp;&nbsp;&nbsp;&nbsp; DNX_IIS_RUNTIME_FRAMEWORK = dnx46<p>![image](https://jcblogimages.blob.core.windows.net/img/2015/07/image25.png "image")

A huge thanks goes out to [Andrew Nurse](https://twitter.com/anurse) for providing a resolution on [this matter](http://stackoverflow.com/questions/31671851/vs-2015-setting-right-target-framework-for-asp-net-5-web-project/31687529#31687529) and responding to [my issue](https://github.com/aspnet/dnx/issues/2367) on GitHub.