---
title: Who said programmers are un-date-able ?
author: Rob
layout: post
permalink: /archives/who-said-programmers-are-un-date-able/
dsq_thread_id:
  - 891142740
categories:
  - SQL Server
---
# 

**First day of the month:**

`DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,0,GETDATE()))`

**First day of the week:**

`DATEADD(yyyy,&nbsp;DATEPART(yyyy,&nbsp;DATEADD(weekday,1-DATEPART(weekday,&nbsp;GETDATE()),GETDATE()))-1900,&nbsp;0)&nbsp; &nbsp;DATEADD(dy,&nbsp;DATEPART(dy,&nbsp;DATEADD(weekday,1-DATEPART(weekday,&nbsp;GETDATE()),GETDATE()))-1,0)`

**Last day of the month:**

`DATEADD(ms,&nbsp;-3,&nbsp;DATEADD&nbsp;(m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0))`

**Last Business Day of the Month:  (buckle up for this one)  
**

`CASE&nbsp;WHEN&nbsp;DATEPART(DW,&nbsp;DATEADD(ms,&nbsp;-3,&nbsp;DATEADD&nbsp;(m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0)))=7&nbsp;`

`THEN&nbsp;DATEADD(DAY,-2,&nbsp;DATEADD(ms,&nbsp;-3,&nbsp;DATEADD&nbsp;(m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0))) &nbsp;&nbsp;&nbsp;&nbsp;WHEN&nbsp;DATEPART(DW,&nbsp;DATEADD(ms,&nbsp;-3,&nbsp;DATEADD&nbsp;(m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0)))=6&nbsp;`

`THEN&nbsp;DATEADD(DAY,-1,&nbsp;DATEADD(ms,&nbsp;-3,&nbsp;DATEADD&nbsp;(m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0))) &nbsp;&nbsp;&nbsp;&nbsp;ELSE&nbsp;DATEADD(ms,&nbsp;-3,&nbsp;DATEADD&nbsp;(m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0))&nbsp;END&nbsp;AS&nbsp;Last_Business_day_of_month&nbsp; `

  


**What can I do with this you ask? Well, You can put it in a procedure for SSRS reports to get a dynamic date on subscriptions with a hidden parameter.**

`ALTER&nbsp;PROCEDURE&nbsp;usp_CommonDates_Report AS BEGIN 
SET&nbsp;DATEFIRST&nbsp;1 DECLARE&nbsp;@Now&nbsp;DATETIME &nbsp;&nbsp;&nbsp;&nbsp;,@LDOTM&nbsp;DATETIME&nbsp;--Last&nbsp;Day&nbsp;Of&nbsp;The&nbsp;Month 
SET&nbsp;@Now=&nbsp;GETDATE() SET&nbsp;@LDOTM&nbsp;=&nbsp;DATEADD(ms,&nbsp;-3,&nbsp;DATEADD&nbsp;(m,DATEDIFF(m,0,DATEADD(m,1,@Now)),0)) 
SELECT @Now&nbsp;AS&nbsp;Today ,&nbsp;DATEADD(yyyy,&nbsp;DATEPART(yyyy,&nbsp;DATEADD(weekday,1-DATEPART(weekday,&nbsp;@Now),@Now))-1900,&nbsp;0)&nbsp; &nbsp;DATEADD(dy,&nbsp;DATEPART(dy,&nbsp;DATEADD(weekday,1-DATEPART(weekday,&nbsp;@Now),@Now))-1,0)&nbsp;AS&nbsp;Week_Start ,&nbsp;@LDOTM&nbsp;AS&nbsp;Last_day_of_month 
,&nbsp;CASE&nbsp;WHEN&nbsp;DATEPART(DW,&nbsp;@LDOTM)=7&nbsp;THEN&nbsp;DATEADD(DAY,-2,&nbsp;@LDOTM) &nbsp;&nbsp;&nbsp;&nbsp;WHEN&nbsp;DATEPART(DW,&nbsp;@LDOTM)=6&nbsp;THEN&nbsp;DATEADD(DAY,-1,&nbsp;@LDOTM) &nbsp;&nbsp;&nbsp;&nbsp;ELSE&nbsp;@LDOTM&nbsp;END&nbsp;AS&nbsp;Last_Business_day_of_month END`