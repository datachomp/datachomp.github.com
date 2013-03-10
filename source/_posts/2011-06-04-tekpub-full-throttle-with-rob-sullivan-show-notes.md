---
title: 'Tekpub &#8211; Full Throttle with Rob Sullivan show notes'
author: Rob
layout: post
permalink: /archives/tekpub-full-throttle-with-rob-sullivan-show-notes/
dsq_thread_id:
  - 888404977
categories:
  - AppDev
  - Databases
  - MVC
  - SQL Server
  - Tools
---
# 

The question emails are starting to pile in (which is a good thing!) so this post is basically a way to centralize links/answers. In the show, I say a few times "we'll see this later" (like with SQL Server Profiler) and we never see it... that was mostly my fault. For some reason I kept thinking I would be able to squeeze 5 hours of content into 1 hour and was wrong. If there are aspects of SQL Server that you would like to see more of, as well as real world problems that you may have and would like to see solutions to, then let [Tekpub know][1]. We'll see if we can hack out real world solutions wrapped in a warm blanket of statistics and opinion.

 [1]: http://help.tekpub.com/

**Questions:***  
**How do you get the stats to appear in the "messages" window?**  
Those stats come from the following commands:  
SET STATISTICS IO ON;  
SET STATISTICS TIME ON;  
If you don't want to turn it on for every query/window, you can do what I do and set them to always be on in Management studio, you can do this by :  
Tools -> Options -> Query Execution -> SQL Server -> Advanced  
then check:  
SET STATISTICS IO ON;  
SET STATISTICS TIME ON; 
**Lock Pages in Memory, is that always good?**  
No, and perhaps I glossed over that section too much or technology changes too fast. While Lock Pages in Memory can be a wonderful setting, there are of course times when it can be pretty bad. You can read more about that here: http://sqlserverperformance.wordpress.com/2011/02/14/sql-server-and-the-lock-pages-in-memory-right-in-windows-server/ 
**Why no filtered index on Posts?**  
In our first scenario, to get the front page up... we just went with the index that the engine suggested.. That got us up and running which is great, but there is room for improvement. To me, filtered indexes are more of a business rule decision and with enough time, if we saw that the load was heavy and predictable enough, we would have likely thrown a filtered index to satisfy the front page query.

**What were you using for "Intellisense" in management studio?** I am not extremely big on intellisense in general, however, I am kind of big on formatting and having a variety of administrative snippets at my finger tips. I use a 3rd party tool called Red-Gate SQL Prompt and have it moderately customized. Keep in mind, I really only use this type of product on data sets I am not very familiar with like the one in this demo. On datasets that I know pretty well, I find intellisense to really just get in the way and make me angry.

**And the database?** I am still working on a way to make the DB available. It is roughly 40GB raw, and 4GB compressed. If having the data is a big deal to you, hit me up on email and we can work something out (S3,FTP, yada yada yada)

**Tech used in the series:**  
Visual Studio 2010 / MVC3 / EF 4.1  
SQL Server 2008 R2 / Management Studio  
[Red Gate SQL Prompt][2] "Intellisense" and "snippets" in Management Studio  
[SQL Sentry Plan Explorer][3] The fun/effective way to view execution plans  
[EF Profiler][4] The SQL Server friendly way to profile EF  
[CPU-Z][5] A tried and true hardware enthusiasts way of checking hardware specs.

**Source code:** 

 [2]: http://www.red-gate.com/products/sql-development/sql-prompt/
 [3]: http://www.sqlsentry.com/plan-explorer/sql-server-query-view.asp
 [4]: http://efprof.com/
 [5]: http://www.cpuid.com/softwares/cpu-z.html