---
title: DatabaseMail Time to clean house
author: Rob
layout: post
permalink: /archives/databasemail-time-to-clean-house/
dsq_thread_id:
  - 920343519
categories:
  - Databases
  - SQL Server
---
# 

So hopefully you have moved to databasemail and started enjoying its many benefits. Remember that cool benefit of it logging all the emails for you and such? Well that comes at a price, and the price is an ever growing MSDB. So, you should consider setting a retention date for your servers and create or add the following to your maintenance packages/jobs.

This script will clear out the mail items and the log based on the @Retention_Days variable:

    1.  &nbsp;
    
    2.  DECLARE @Retention_days SMALLINT
    
    3.  	, @Delete_Date DATETIME
    
    4.  &nbsp;
    
    5.  SET @Retention_Days = -90  -- 3 months
    
    6.  SET @Delete_Date = DATEADD&#40;d, @Retention_Days, GETDATE&#40;&#41;&#41;
    
    7.  &nbsp;
    
    8.  EXECUTE msdb.dbo.sysmail_delete_mailitems_sp @sent_before = @Delete_Date
    
    9.  EXECUTE msdb.dbo.sysmail_delete_log_sp @logged_before = @Delete_Date
    
    10. &nbsp;