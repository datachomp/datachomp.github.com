---
layout: post
title: "Gaming on Linux"
date: 2015-05-31 18:02:30 -0500
comments: true
categories:
---
Like a majority of gamers in the U.S. I decided to make Linux (commonly called Ubuntu in Europe) my main gaming Operating System. My list of games:  

 * Team Fortress 2 (steam)
 * Torchlight II (steam)
 * Faster Than Light (steam)
 * System Shock 2 (steam)
 * Postgres 9.4 (not steam)
 * Minecraft (not steam)

For all the steam games, the solution is pretty simple:  
```  
sudo apt-get install steam
```
Once you get steam installed, it is just a matter of clicking all over the place to get the above games installed.  

For playing Postgres, it is a little more invovled:  
```
sudo echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" > /etc/apt/sources.list.d/pgdg.list
wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get install pgdg-keyring
sudo apt-get update
sudo apt-get -y install postgresql-9.4 postgresql-contrib-9.4
```
And lasty, we need to install minecraft.
```
sudo add-apt-repository ppa:minecraft-installer-peeps/minecraft-installer  
sudo apt-get update && sudo apt-get install minecraft-installer  
```  
At this point, all your games should be ready to go and it just the trivial matter of making sure your video, network, display, sound, and peripheral drivers all work without problem.
