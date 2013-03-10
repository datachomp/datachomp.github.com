---
title: TempDB on the move
author: Rob
layout: post
permalink: /archives/tempdb-on-the-move/
dsq_thread_id:
  - 894650114
categories:
  - SQL Server
---
# 

`/************************************************************************ *By&nbsp;default,&nbsp;your&nbsp;tempDB&nbsp;files&nbsp;are&nbsp;in&nbsp;the&nbsp;following&nbsp;locations: *C:Program&nbsp;FilesMicrosoft&nbsp;SQL&nbsp;ServerMSSQL.1MSSQLDatatempdb.mdf *C:Program&nbsp;FilesMicrosoft&nbsp;SQL&nbsp;ServerMSSQL.1MSSQLDatatempdb.ldf * *The&nbsp;below&nbsp;commands&nbsp;will&nbsp;change&nbsp;the&nbsp;default&nbsp;tempDB&nbsp;locations&nbsp;for&nbsp;SQL&nbsp;Server *and&nbsp;have&nbsp;it&nbsp;create&nbsp;the&nbsp;tempDB&nbsp;files&nbsp;there&nbsp;upon&nbsp;start&nbsp;up. ************************************************************************/ USE&nbsp;master GO 
ALTER&nbsp;DATABASE&nbsp;tempdb&nbsp;MODIFY&nbsp;FILE&nbsp;(NAME&nbsp;=&nbsp;tempdev,&nbsp;FILENAME&nbsp;=&nbsp;'E:Datatempdb.mdf') GO ALTER&nbsp;DATABASE&nbsp;tempdb&nbsp;MODIFY&nbsp;FILE&nbsp;(NAME&nbsp;=&nbsp;templog,&nbsp;FILENAME&nbsp;=&nbsp;'F:Logtemplog.ldf') GO&nbsp; 
/************************************************************************ *You&nbsp;will&nbsp;need&nbsp;to&nbsp;restart&nbsp;SQL&nbsp;Server&nbsp;for&nbsp;tempdb's&nbsp;file&nbsp;to&nbsp;actually&nbsp;move. * *More&nbsp;reading&nbsp;on&nbsp;the&nbsp;topic: *http://www.databasejournal.com/features/mssql/article.php/3379901 ************************************************************************/ `