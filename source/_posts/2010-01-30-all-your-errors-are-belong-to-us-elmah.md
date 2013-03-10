---
title: 'All your errors are belong to us &#8211; ELMAH'
author: Rob
layout: post
permalink: /archives/all-your-errors-are-belong-to-us-elmah/
dsq_thread_id:
  - 917205910
categories:
  - ASP.NET
  - Tools
---
# 

So I've been working on a step by step on ELMAH and how to set it up for F5Compliant ... when today I came across Scott Mitchels tutorial on [ASP.NET][1]

 [1]: http://asp.net/



One thing he seems to have left out is using SQLite.  Since moving from my in house servers to online hosting, I am still often surprised by the DB limitations faced.  Before I went with a hosted solution, it just felt commonplace to spin up a new DB for X project and go.   With many shared hosting plans, one becomes a little miserly with their DB's as more DB's with the hosting plan incur more cost.   SQLite seems to fill that void quite nicely.  One thing to be aware of though is that SQLite requires full trust in order to run, so you will probably need to contact your hosting provider for them to set the permissions.   I use SoftSysHosting and their team takes care of this incredibly fast which is nice.