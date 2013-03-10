---
title: GIT and Red Gate SQL Source Control
author: Rob
layout: post
permalink: /archives/git-and-red-gate-sql-source-control/
dsq_thread_id:
  - 889168841
categories:
  - Databases
  - SQL Server
---
# 

I am a huge fan of [Red Gate SQL Source Control][1] because it makes source controlling/deploying your database easier. I am aware of the MS Database Projects abomination application and the short version on that is that I have deemed them unworthy for my use. There are lots of tutorials and examples of using RG SSC with centralized version control systems like [Subversion][2] or [TFS][3] and it does work wonderfully with them. Recently, my workplace decided to ditch TFS and move to a more modern distributed version control named [Git][4]. Like most disruptions to your workflow, there are a few growing pains. This post is going to cover installing Git on Widows, hooking it up to [github.com][5] and showing some basic actions as we change our database.

 [1]: http://www.red-gate.com/products/sql-development/sql-source-control/
 [2]: http://subversion.tigris.org/
 [3]: http://en.wikipedia.org/wiki/Nelson_Muntz
 [4]: http://git-scm.com/
 [5]: https://github.com/

**Install Git:**  
Download the [full installer for Git][6] and install it.  
While you are mindlessly clicking 'next' make sure you integrate with CMD. This is critical for RG SSC and just a convenient thing to do in general. The important part will look like this:  
[][7]

 [6]: http://code.google.com/p/msysgit/downloads/list?can=3
 [7]: http://files.datachomp.com/SQLServer/rgssc/1install_cmd.png

**Aside:** I'm a converted fan of [posh-git][8]. Using things like the Git GUI or Tortoise GIT will be incredibly tempting, and perhaps for initial familiarization, it might be a good idea to use those to visualize some concepts. That being said, it is really in your best interest to move towards the CLI and posh-git is very helpful in that regard. It isn't nearly as bad as you think once you get going.

 [8]: https://github.com/dahlbyk/posh-git

**Repo Hosting**  
Once you have Git installed, you will want to hook up to some hosting. An incredibly popular and easy to use one is [Github.com][5] but there are [plenty of others][9].

 [9]: http://git-scm.com/tools

**Repo Man**  
In Github, you will want to create your repository and [assign your SSH keys][10]. This process can be a bit more than you are used to at first, but GitHub as great at giving you some hand holding instructions:   
Once the repo is initialized, I like to create a folder for the App and a folder for the database like so:  
[][11]  
You don't have to do it like that, but again, that is the way I currently like to do it.

 [10]: http://help.github.com/win-set-up-git/
 [11]: http://files.datachomp.com/SQLServer/rgssc/2folder_layout.png "folder layout"

**Lets hook up the DB!**  
Why use the Adventure Works database? Since it is a bit of a marketing database used to show both neat and inane features, we're hooking it up just to make sure all types of various objects work just fine with RG SQL Source Control.  
From there, go to SSMS and link up the DB to your local repository:  
[][12] [][13]

 [12]: http://files.datachomp.com/SQLServer/rgssc/4hookupssc.png "repo linko"
 [13]: http://files.datachomp.com/SQLServer/rgssc/5configureforgit.png "repo linko"

**Command Line Interface ... or whatever**  
Back in powershell (or whatever client you are using) create a new branch for updating our proc. `git branch dbobjects`  
Note: We don't have to create a new branch. We could very easily just keep doing this in the 'master' branch. I find that creating of the branches to be a good habit and a handy organizational tool. Branching is one of those features that really shines in Git so lets put it to use.  
Go into that branch: `git checkout dbobjects`  
[][14]

 [14]: http://files.datachomp.com/SQLServer/rgssc/6createbranch.png "branchard"

Now, we go back to SSMS and commit our changes:  
[][15]

 [15]: http://files.datachomp.com/SQLServer/rgssc/7addobjects.png "no hands!"

Cool! Our "dbobjects" branch has been committed locally, now we just need to merge it into our master branch!

From there, go back to master: `git checkout master`  
and then merge into master our dbobjects branch: `git merge master dbobjects`  
After we have merged, we can delete the branch we were working in by running:  
`git branch -d dbobjects`  
and the process will end up looking like this:  
[][16]

 [16]: http://files.datachomp.com/SQLServer/rgssc/9gotomasterandmerge.png "master merge"

If you look on Github, you will see that nothing has changed there. Why is that? That is because all the work we have done has been locally. This is the essence and the speed of a distributed control system. We can commit, fix, break all we want without getting everyone else mad. However, at some point, we will need to send our changes off so that others on the team can use (laugh) at them. We do this by "push"ing to the origin: `git push -u origin master`  
[][17]

 [17]: http://files.datachomp.com/SQLServer/rgssc/10pushtoorigin.png "origin"

Now, if we look at Github, we see our changes in the main repository, and if we go to check it out again:  
[][18]

 [18]: http://files.datachomp.com/SQLServer/rgssc/11alldoneyay.png "yay"

We can see them there for people pulling down changes. You've done it. Now you too can participate in the source control fun with the AppDevs and make nerdworthy comments about rebasing, merging and pushing master.