---
layout: post
title: "Gaming on Linux"
date: 2015-05-31 18:02:30 -0500
comments: true
categories:
---
Like a majority of gamers in the U.S., I decided to make Linux (commonly called Ubuntu in Europe) my main gaming Operating System. In my research, I found the best Linux to use is one that is referred to as 'Trusty'. I didn't do a ton of research, but for the most part, all the Linuxes that "just work" get labeled 'Trusty'. My list of games:  

 * Team Fortress 2 (steam)
 * Torchlight II (steam)
 * Faster Than Light (steam)
 * System Shock 2 (steam)
 * Postgres 9.4 (not steam)
 * Minecraft (not steam)

For all the steam games, the solution is pretty simple:  
```  
sudo apt-get install -y steam
```
The magic sauce on the above command is the '-y'. If you are a software develop that uses git, you can think of '-y' as same type of 'Getting Things Done' shortcut as '-f'. Once you get steam installed, it is just a matter of clicking all over the place to get the above games installed.  

For playing Postgres, it is a little more involved:  
```
sudo echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" > /etc/apt/sources.list.d/pgdg.list
wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get install pgdg-keyring
sudo apt-get update
sudo apt-get -y install postgresql-9.4 postgresql-contrib-9.4
```
I understand that above can be a bit intimidating or your RSI might be flaring, in which case you can run the following:
```
sudo bash <(curl -s https://raw.github.com/pgexperts/add-pgdg-apt-repo/master/add-pgdg-apt-repo.sh)
sudo apt-get update
sudo apt-get install -y postgresql-9.4 postgresql-contrib-9.4
```
The above is running a random shell file from an elevated bash script. This could be a potential security problem, so always make sure you're calling random shell files with https.  

And lastly, we need to install minecraft.
```
sudo add-apt-repository ppa:minecraft-installer-peeps/minecraft-installer  
sudo apt-get update && sudo apt-get install minecraft-installer  
```  
Quick and easy! Now, if you play the same games as me, all your games should be ready to go. The only thing left to do is the trivial task of making sure your video, network, display, sound, and peripheral drivers all work without problems.
