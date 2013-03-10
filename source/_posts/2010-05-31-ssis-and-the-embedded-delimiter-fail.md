---
title: SSIS and the embedded delimiter fail
author: Rob
layout: post
permalink: /archives/ssis-and-the-embedded-delimiter-fail/
dsq_thread_id:
  - 894113402
categories:
  - Databases
  - SQL Server
---
# 

Simple CSV file with some embedded delimiters. No big deal in SQL 2000 DTS right? Well, it's a huge deal in SQL Server 2005 and SQL Server 2005 SSIS. Of course, it is a bit of a mystery while they keep removing functionality but that isn't the point of this post. The point of this post is to show a solution...

The solution comes by way of the Microsoft SQL Server Community Samples: Integration Services (yes, it is a long name for 'Look dudes! We are giving you a work around!' but it works non the less).

http://sqlsrvintegrationsrv.codeplex.com/

I tried the delimited file reader in a variety of scenarios and it passed with flying colors. How nice it is to be able do the same imports that were effortless in SQL 2000.