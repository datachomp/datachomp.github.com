---
title: Getting Started with Massive in MVC3
author: Rob
layout: post
permalink: /archives/getting-started-with-massive-in-mvc3/
dsq_thread_id:
  - 888404945
categories:
  - AppDev
  - MVC
---
# 

[Rob Conery][1] has created a new ORM of sorts   and as a DBA, I must say I like what I see so far.

 [1]: http://blog.wekeroad.com/

Getting started:  So some of you might be noob programmers like myself and after adding Massive to your MVC application via Nuget you become lost in space because it doesn't work.  The fix to this is rather easy,  just check the properties of the  Massive.cs file and make sure its build action is set to Compile... you might want to change the namespace as well depending on how picky you are with namespace juggling.

We DBA's are a particularly picky bunch and one of the things we hate about ORM's  is something called 'plan cache poisoning'.  Want to know why your DBA hated Linq to SQL until  .NET 4 came out?  One of the reasons is 'plan cache poisoning'  which I will call PCP for short.   When you make a query to the database SQL Server compiles the that query into a plan so that the next time you go to run  it... it doesn't have to compile a new plan... it just pulls from its cache.  When it comes to  linq-sql and EF before  .net 4....  they would pass a new string length depending upon the size of the string you are looking for.  So, if you have a table of contacts and want to pull someone up by first name...  it would compile a plan for 'Rob', a plan for 'Robert',  a plan for 'Mike' and so forth until you have exhausted all the different name lengths.    This sucks.   With Massive your  DBA won't wake up in cold sweats thinking about his beloved plan cache because of your ORM.

To make  basic call with Massive on a Drinks Table (DrinkID, DrinkName) we do this:

    1.  var table = [new][2] Drinks&#40;&#41;;
    
    2.  var drinks = table.All&#40;&#41;;

 [2]: http://www.google.com/search?q=new msdn.microsoft.com

If you have done any Linq or EF you know that even a basic call like that is going to cause Linq or EF to create  
the ugliest looking t-sql it can by breaking out into extent aliases God knows what else it likes to do to say 'hey look at me'  
Does Massive have this problem? Nope, looking at profiler we get straight forward SQL -

    1.  SELECT * FROM DRINKS

Well, that's cool and all if you want to start barfing out tables into your app, but how often to get to exercise such negligence?  
Rarely, so lets move to some conditions... lets look up a drink name:

    1.  var table = [new][2] Drinks&#40;&#41;;
    
    2.  var drinks = table.All&#40;where: "WHERE DrinkName=@0", args: "Fire Rock Pale Ale"&#41;;

Knowing EF, we're likely going to have 15 lines of aliases and subqueries to run this most complicated of queries..  
In Massive, we get:

    1.  EXEC SP_EXECUTESQL N'SELECT * FROM Drinks WHERE DrinkName=@0',N'@0 nvarchar(4000)',@=N'Fire Rock Pale Ale'

In addition to a simple to grok TSQL, we also start getting reusable query plans.

More examples to come as I start to peel back the covers a bit on this new tool.