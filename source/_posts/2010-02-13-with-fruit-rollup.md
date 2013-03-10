---
title: With Fruit Rollup
author: Rob
layout: post
permalink: /archives/with-fruit-rollup/
dsq_thread_id:
  - 893414065
categories:
  - Databases
  - SQL Server
---
# 

Anyone who has to do reporting on a recurring basis knows what a pain it can be when you have to provide columns totals for the app. Treatment for this pain can be found with the ROLLUP command.  
Run this example to see what I mean:

CREATE TABLE #temper (row_id INT IDENTITY(1,1)  
   , Account_Name VARCHAR(50), balance smallmoney);  
INSERT INTO #temper   
VALUES ('Jimmy Johns', 50),('Qdoba',60)  
   ,('Big Truck Tacos', 70),('Hideaway', 80),('Teds', 90);  
SELECT Account_Name, SUM(balance) AS BALANCE  
FROM #temper  
GROUP BY Account_Name WITH ROLLUP;  
DROP TABLE #temper;

Pretty cool right?  The downside being that there is a NULL to deal with and that isn't a very descript name for the total.  So maybe we plaster on a CASE statement to replace the NULL with something a little digestible to a user:

CREATE TABLE #temper (row_id INT IDENTITY(1,1)  
   , Account_Name VARCHAR(50), balance smallmoney);  
INSERT INTO #temper   
VALUES ('Jimmy Johns', 50),('Qdoba',60)  
   ,('Big Truck Tacos', 70),('Hideaway', 80),('Teds', 90);  
SELECT CASE  
   WHEN (Grouping(Account_Name)=1) THEN 'Total'  
   ELSE Account_Name  
   END AS Account_Name  
   , SUM(balance) AS BALANCE  
FROM #temper  
GROUP BY Account_Name WITH ROLLUP;  
DROP TABLE #temper;

For more a more wordy version of this, consult the 4 Guys:  
