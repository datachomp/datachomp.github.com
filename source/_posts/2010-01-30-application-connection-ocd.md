---
title: Application Connection OCD
author: Rob
layout: post
permalink: /archives/application-connection-ocd/
dsq_thread_id:
  - 890457085
categories:
  - AppDev
---
# 

Do you have this problem:

 

![][1]

 [1]: http://images.datachomp.com/AppDev/ApplicationConnectionOCD.png

Multiple connections from various applications and you don't know which is which.  There is a fix to this.  It involves telling the app dev team to create a nice clean connection string with the Application Name property being set. This gets double awesome when you add the hostname to the application name so you know exactly where in the webfarm something is coming from.

Here is an example:

`  `