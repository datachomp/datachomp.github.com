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
<code>
	DATEADD(dd,-(DAY(DATEADD(mm,1,GETDATE()))-1),DATEADD(mm,0,GETDATE()))
</code>
**First day of the week:**
<code>
	DATEADD(yyyy, DATEPART(yyyy, DATEADD(weekday, 1-DATEPART(weekday, GETDATE()),GETDATE()))-1900, 0) + DATEADD(dy, DATEPART(dy, DATEADD(weekday,	1-DATEPART(weekday, GETDATE()), GETDATE()))-1,0)
</code>
**Last day of the month:**
<code>
	DATEADD(ms, -3, DATEADD (m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0))
</code>
**Last Business Day of the Month:  (buckle up for this one)  
**
<code>
	CASE WHEN DATEPART(DW, DATEADD(ms, -3, DATEADD (m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0)))=7 
	THEN DATEADD(DAY,-2, DATEADD(ms, -3, DATEADD (m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0))) 
	    WHEN DATEPART(DW, DATEADD(ms, -3, DATEADD (m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0)))=6 
	THEN DATEADD(DAY,-1, DATEADD(ms, -3, DATEADD (m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0))) 
	    ELSE DATEADD(ms, -3, DATEADD (m,DATEDIFF(m,0,DATEADD(m,1,GETDATE())),0)) END AS Last_Business_day_of_month 

</code>
  


**What can I do with this you ask? Well, You can put it in a procedure for SSRS reports to get a dynamic date on subscriptions with a hidden parameter.**
<code>
	ALTER PROCEDURE usp_CommonDates_Report 
	AS 
	BEGIN
	SET DATEFIRST 1 
	DECLARE @Now DATETIME 
	    ,@LDOTM DATETIME --Last Day Of The Month
	SET @Now= GETDATE() 
	SET @LDOTM = DATEADD(ms, -3, DATEADD (m,DATEDIFF(m,0,DATEADD(m,1,@Now)),0))
	SELECT 
	@Now AS Today 
	, DATEADD(yyyy, DATEPART(yyyy, DATEADD(weekday,1-DATEPART(weekday, @Now),@Now))-1900, 0) + DATEADD(dy, DATEPART(dy, DATEADD(weekday,1-DATEPART(weekday, @Now),@Now))-1,0) AS Week_Start 
	, @LDOTM AS Last_day_of_month
	, CASE WHEN DATEPART(DW, @LDOTM)=7 THEN DATEADD(DAY,-2, @LDOTM) 
	    WHEN DATEPART(DW, @LDOTM)=6 THEN DATEADD(DAY,-1, @LDOTM) 
	    ELSE @LDOTM END AS Last_Business_day_of_month 
	END
</code>