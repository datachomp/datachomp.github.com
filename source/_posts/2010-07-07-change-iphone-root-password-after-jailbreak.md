---
title: Change iPhone root password after jailbreak
author: Rob
layout: post
permalink: /archives/change-iphone-root-password-after-jailbreak/
dsq_thread_id:
  - 892979936
categories:
  - iPhone
  - Security
---
# 

One of the great benefits to having a jailbroken iPhone is the SSH feature. One of the great security risks of having a jailbroken iPhone is the SSH feature and the fact that the phone has the same global password for all iPhones. Luckily, there is a quick fix for this.

In Cydia, go to the "Search" section and find the app called ***MobileTerminal***. Install it, then launch it.  
From there, type in ***su root*** and hit return. This changes us to the root user so we can access/change the password for it. It will then prompt you to type in that global root password of ***alpine*** . Once you are root, just type ***passwd*** then hit return. From there, you will need to enter, then re-enter your password and the change should take place. Simply type in ***exit*** to leave the root account and get back to the standard prompt.

**Another Notch: Changing the Mobile User**  
You should also consider doing the same thing for your Mobile user as it uses a global password of "alpine" as well  
You start the MobileTerminal app as the Mobile user, so to change the password for it simply type ***passwd** Then type alpine when it prompts, then fill out the stuff for the new password and start sleeping better at night knowing that you have just put up a basic security fence for your sweet sweet jailbroken phone. 
For more information, check out these handy posts:  
  
and  
