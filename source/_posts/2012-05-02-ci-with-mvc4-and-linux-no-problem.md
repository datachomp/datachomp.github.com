---
title: CI with MVC4 and Linux? No Problem
author: Rob
layout: post
permalink: /archives/ci-with-mvc4-and-linux-no-problem/
dsq_thread_id:
  - 888337443
categories:
  - AppDev
  - Linux
  - MVC
  - Tools
---
# 

There are a lot of tools out there that we can use to automate our awesomeness. One of those easy/free/fun tools is a continuous integration (CI) server. Since we are building in Windows and deploying to Linux, we can go ahead and upgrade a CI server from 'important' to essential. There are various products out there that can accomplish this; I'm going with what I know and that is [TeamCity][1] from JetBrains.

 [1]: http://www.jetbrains.com/teamcity/ "TeamCity"

On our Ubuntu box, after we finish the easy as candy install, we're going to want to install Mono. Check out [this previous post][2] on how to do that and set it up in your /etc/environment.

 [2]: http://datachomp.com/archives/running-asp-net-mvc4-on-ubuntu-12-04/ "Install Mono on Ubuntu"

Now that Mono is rocking, lets hop over to our CI install. At the time of this post, the latest version of TeamCity is 7.0.2a. Looking at the code below, we're going to install Java, pull down the latest version of the TeamCity linux tar, decompress it, switch directories and fire it up :  
<code>
	sudo apt-get install openjdk-7-jre
	wget - http://download.jetbrains.com/teamcity/TeamCity-7.0.2a.tar.gz
	tar xfz TeamCity-7.0.2a.tar.gz
	cd Teamcity
	sudo ./bin/runAll.bat start
</code>

Pretty easy right? Lets check out our web interface:[![TeamcityWelcome][4]][4]

 []: http://files.datachomp.com/AppDev/mono/teamcitywelcome.png
 [4]: http://files.datachomp.com/AppDev/mono/teamcitywelcome.png

Yay! Everything works! You're also at a ubiquitous web interface which will likely give the Softie in you some solace. There are tons of resources online for setting up your project in TeamCity to check it out, build it, run tests, deploy, yada yada yada. If you are just getting started, I highly recommend [Troy Hunt's post.][5] You can also install Ruby on this server and have a wonderfully awesome auto-deployment method via [Capistrano][6]. If you are interested in deploying with Capistrano, let me know I'll try to barf out a quick post on it. This is a pretty short and to the point post, if it needs more content with regards to project checkouts and stuff, let me know and I can barf that out as well.

 [5]: http://www.troyhunt.com/2010/11/you-deploying-it-wrong-teamcity.html "Troy Hunt Deployments"
 [6]: https://github.com/capistrano/capistrano/wiki/Documentation-v2.x "Capistrano"