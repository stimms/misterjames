title: Team Foundation Error – BasicAuthCredential is not marked as serializable
tags:
  - TFS
  - Visual Studio
id: 5601
categories:
  - Develop Meta
date: 2015-01-21 16:43:35
---

I was working on a project hosted in a git repo in Team Foundation and wanted to update the build process. Scroll down for two different fixes if you’ve got the error, or read on to see if it’s applicable to your situation.

When I navigated to the step to configure the process template parameters, I ran into the following error:

![image](https://jcblogimages.blob.core.windows.net/img/2015/01/image4.png "image")

The text was:
 > Team Foundation Error – Type ‘Microsoft.TeamFoundation.Client.BasicAuthCredential’ in assembly ‘Microsoft.TeamFoundation.Client, Version=12.0.0.0, Culture=neutral, PublicKeyToken=’[token] is not marked as serializable. 

I’m not aware of any recent changes or updates to my machine, my dev environment or the TFS server in question. I tried restarting Visual Studio, but the error persisted. 

I then remembered that about three months ago I ran into a similar problem (though I can’t quite recall if it was the same error) when I started using a VM that had a co-worker’s credentials pre-baked into VS. To fix this problem I removed the credentials in Windows, and it resolved the issue, prompting me to sign in as myself when I next launched VS.

## Solution 1 – Reset Your Credentials

Rather than removing the account, I wanted to first try to just poke it by re-entering my password or&nbsp; To reset your credentials, try this:

1.  Close all instances of Visual Studio.
2.  Tap your Windows key, then type “Credentials”. The Credential Manager app link should show up in your list.
3.  Check out your “Windows Credentials”. Under Generic Credentials, you should see your TFS account.
4.  Click on the twirldown next to the entry and select “Edit”.5.  Retype your password. 

## Solution 2 – Delete the Credentials from Windows

[![image](https://jcblogimages.blob.core.windows.net/img/2015/01/image_thumb.png "image")](https://jcblogimages.blob.core.windows.net/img/2015/01/image5.png)If the reset doesn’t work, you can try this instead. 

1.  Close all instances of Visual Studio.
2.  Tap your Windows key, then type “Credentials”. The Credential Manager app link should show up in your list.
3.  Check out your “Windows Credentials”. Under Generic Credentials, you should see your TFS account.
4.  Click on the twirldown next to the entry and select “Delete”.5.  You may need to do this for more than one account, especially if you have a git:http:// account saved as well.
6.  Confirm the delete operation. 

Whichever solution you have to go with, this should fix things up. Note that in solution 2, you’ll be prompted for your credentials when you open up Visual Studio and try to navigate to a team project.

Cheers, and happy coding! ![Smile](https://jcblogimages.blob.core.windows.net/img/2015/01/wlEmoticon-smile.png)
