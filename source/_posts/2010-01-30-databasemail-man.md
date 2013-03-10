---
title: DatabaseMail Man
author: Rob
layout: post
permalink: /archives/databasemail-man/
dsq_thread_id:
  - 896237621
categories:
  - SQL Server
---
# 

Compared to email options presented in SQL Server 2000,   Databasemail in Sql Server 2005 rocks.  Here are some implementation examples:

    -- send a normal mail
    EXEC msdb.dbo.sp_send_dbmail
    @Profile_Name = @@ServerName
    ,@recipients='name@domain.com;number@cingularme.com'
    ,@subject = 'Dev Transfer to 19'
    ,@body = 'Task Complete'
    ,@body_format = 'HTML'

-- send with an attachment  
EXEC msdb.dbo.sp\_send\_dbmail  
@Profile_Name = @@SERVERNAME  
,@recipients='name@domain.com'  
,@subject = 'Dev Transfer to 19'  
,@body = 'Task Complete'  
,@body_format = 'HTML'  
,@file_attachments = @SourcePath;

/*  
--Odds and Ends  
If sending to multiple peeps and one fails to send... queued up and resends to all.

I usually make a profile based on the machine name, so that for notifications, I can keep using @@servername and it be portable.

-- SMTP Authentication  
The mail could not be sent to the recipients because of the mail server failure. (Sending Mail using Account 1 (2007-07-02T08:08:10). Exception Message: Cannot send mails to mail server. (Mailbox unavailable. The server response was: 5.7.1 Unable to relay for name@domain.com). )

-- antivirus port 25 block  
The mail could not be sent to the recipients because of the mail server failure. (Sending Mail using Account 1 (2007-06-18T07:15:51). Exception Message: Could not connect to mail server. (An established connection was aborted by the software in your host machine). )

--Select * from sysmail\_allitems order by mailitem\_id desc   -- look up mails  
--Select * from sysmail\_event\_log WHERE mailitem_id in (95,94,93,97) -- look up the mails details  
*/