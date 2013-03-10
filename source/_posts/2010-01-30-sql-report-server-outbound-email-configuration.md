---
title: SQL Report Server Outbound Email Configuration
author: Rob
layout: post
permalink: /archives/sql-report-server-outbound-email-configuration/
dsq_thread_id:
  - 888404933
categories:
  - SQL Server
---
# 

So you get your handy dandy [SQL Server Report Server all set up for email][1] and are looking good then you go to have it email to an outbound address.  All of a sudden you get the 'The e-mail address of one or more recipients is not valid.'   While there could be a couple reasons for this issue, the main two I run into is SMTP authentication and user aliases.  The fix to both of these involves updating the ReportServer.Config file usually found in the filesystem with a folder structure resembling - "c:program filesmicrosoft SQL ServerMSRS10.MSSQLSERVER"

 [1]: http://technet.microsoft.com/en-us/library/ms345234.aspx

You want to pop it open and then search for "SMTPServer" which will take you to something that looks like this:

*  
[email.example.com][2]  
  
  
  
  
  
2  
0  
[MrMail@example.com][3]  
MHTML  
  
HTMLOWC  
NULLRGDI  
  
True  
Example.com  
  
* 
The two fields you want to focus on are:

0 and True



You will want to change these to

**2** and **False**



This will let use the report servers service credentials to authenticate to the SMTP server. In addition, Report Server start using the actual email address instead of trying to turn everything into an alias depending on what you have populated for DefaultHostName.

 [2]: http://email.example.com/
 [3]: mailto:PikepassMail@pikepass.com

For more information about the report server configuration settings,  see here: [http://msdn.microsoft.com/en-us/library/ms157273.aspx][4]

This post also makes the assumption you do SMTP authenticate for outbound emails.... If you are unsure, then double check that you are not currently operating an open relay email server.

 [4]: http://msdn.microsoft.com/en-us/library/ms157273.aspx "ReportServer Configuration File"