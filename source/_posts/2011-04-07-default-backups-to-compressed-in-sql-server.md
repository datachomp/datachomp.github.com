---
title: Default backup compression in SQL Server
author: Rob
layout: post
permalink: /archives/default-backups-to-compressed-in-sql-server/
dsq_thread_id:
  - 901042407
categories:
  - Databases
  - SQL Server
---
# 

I always forget this so it is going in a blog for quick reference:

    1.  &nbsp;
    
    2.  SP_CONFIGURE 'show advanced options',1
    
    3.  RECONFIGURE WITH override
    
    4.  GO
    
    5.  SP_CONFIGURE 'backup compression default',1
    
    6.  RECONFIGURE WITH override
    
    7.  GO
    
    8.  &nbsp;