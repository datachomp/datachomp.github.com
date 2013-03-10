---
title: Installing MongoDB on Ubuntu
author: Rob
layout: post
permalink: /archives/installing-mongodb-on-ubuntu/
dsq_thread_id:
  - 891946093
categories:
  - Databases
  - Linux
  - NoSQL
  - OS
---
# 

First step to getting started with toying around with NoSQL is getting it running. Currently, MongoDB is my choice in the land of learning. To get it going on my VM's I do the following after my Ubuntu server is installed.

Navigate to: /etc/apt/sources.list  
Then: sudo pico sources.list  
In your sources, add the following line: deb http://downloads.mongodb.org/distros/ubuntu 10.4 10gen  
Save/exit the file, then add the key: sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10

Once that is done, you're ready to update and install.  
Update: sudo apt-get update  
Install: sudo apt-get install mongodb-stable

Well now what?  
If you need to configure MongoDB further, then navigate to: /etc/mongodb.conf  
If you need to add/remove/mess with the database, it lives in: /var/lib/mongodb  
Want the logs? They are here: /var/log/mongodb/ (.log extension)