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
<code>
	SP_CONFIGURE 'show advanced options',1
	RECONFIGURE WITH override
	GO
	    
	SP_CONFIGURE 'backup compression default',1
	RECONFIGURE WITH override
	GO
</code>