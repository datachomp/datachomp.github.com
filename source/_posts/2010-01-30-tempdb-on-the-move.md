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
<code>
By default, your tempDB files are in the following locations:    
C:Program FilesMicrosoft SQL ServerMSSQL.1MSSQLDatatempdb.mdf   
C:Program FilesMicrosoft SQL ServerMSSQL.1MSSQLDatatempdb.ldf  
   
The below commands will change the default tempDB locations for SQL Server   
and have it create the tempDB files there upon start up.   
</code>
<code>
USE master   
GO   
ALTER DATABASE tempdb MODIFY FILE (NAME = tempdev, FILENAME = 'E:Datatempdb.mdf')    
GO    
ALTER DATABASE tempdb MODIFY FILE (NAME = templog, FILENAME = 'F:Logtemplog.ldf')    
GO
</code>
<code>
You will need to restart SQL Server for tempdb's file to actually move.    
     
More reading on the topic:    
http://www.databasejournal.com/features/mssql/article.php/3379901    
</code>