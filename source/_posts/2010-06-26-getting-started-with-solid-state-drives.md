---
title: Getting started with Solid State Drives
author: Rob
layout: post
permalink: /archives/getting-started-with-solid-state-drives/
dsq_thread_id:
  - 894992709
categories:
  - OS
---
# 

Step 1 - Make sure AHCI is working in the bios. This is used for TRIM and such.  
Step 2 - Make sure the SSD is aligned. Pull up "msinfo32", navigate to Components -> Storage -> Disks, and look for "Partition Starting Offset". Use those numbers and [Techpowerup - SSD Alignment Calculator][1] to make sure your disk is aligned.

 [1]: http://www.techpowerup.com/articles/other/157

Step 3 - Make sure TRIM is enabled. At the command prompt, type-> "fsutil behavior query disabledeletenotify" A 0 means that TRIM is on, a 1 means it is off. To turn Trim on type, "fsutil behavior set disabledeletenotify 0" .

Step 4 - Use programs like Filemon and such to see if you have something that is doing heavy writes on your SSD. Depending on what they are, you might want to move them to something else.