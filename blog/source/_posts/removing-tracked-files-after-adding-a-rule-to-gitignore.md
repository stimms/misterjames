title: Removing Tracked Files After Adding A Rule to .gitignore
tags:
  - Git
id: 731
categories:
  - Code Dive
date: 2012-05-10 17:52:00
---

As I learn some of the workings of git in the normal flow of daily use, I struggled for a few minutes this afternoon trying to scrub a directory of DLLs that had been committed to a repo.&nbsp; These files were previously tracked, so even though the rule was correct, git wasn’t dropping tracking to the files.

Here’s the fix from the mouth of the horse:
 > #### .gitignore
> 
> If you create a file in your repo named `.gitignore` git will use its rules when looking at files to commit. Note that git will **not** ignore a file that was already tracked before a rule was added to this file to ignore it. In such a case the file must be un-tracked, usually with `git rm --cached filename` 

There ya be.&nbsp; So, create your rule, rm the files and _then_ do your git status to see the changes.&nbsp; The removing them from tracking is also a change that will have to be committed.