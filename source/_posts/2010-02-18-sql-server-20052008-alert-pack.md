---
title: SQL Server 2005/2008 Alert Pack
author: Rob
layout: post
permalink: /archives/sql-server-20052008-alert-pack/
dsq_thread_id:
  - 889849483
categories:
  - Databases
---
# 

Outside of the daily WTF's I create... one of the larger WTF's I know is the SQL Server team removing the built in alerts in SQL Server 2005 and SQL Server 2008. Naturally I am going to gloss over a lot of this and just link up the code. To go more in depth on this, read the 'related link' at the bottom.

Lets walk through the file- (Make sure you have DatabaseMail set up and set the Agent to use DatabaseMail)

1.  In the @email list variable... set the notification emails you want
2.  Notice the 'use MSDB' - that is SQL Agent's crib
3.  You are then going to see the create statements for a variety of alerts in the agent
*   the SQL 2000 alerts have some different delay intervals that throw warnings in 05/08. my script fixes that

4.  Create an operator named 'DBA' and assign it the desired emails.
5.  Add the DBA operator to the alerts and tell it to use email notification

Here is the file:  


Related Link:  
[http://www.karaszi.com/SQLServer/util\_agent\_alerts.asp][1]

 [1]: http://www.karaszi.com/SQLServer/util_agent_alerts.asp