---
title: Getting Started with SQL CLR Part 1
author: Rob
excerpt: 'Start: Anyone who has done scripting tasks in SSIS knows how valuable being able to leverage .NET can be when you are building data solutions. Based on a presentation I saw, by Tim Mitchell, I was encouraged to extend .NET past SSIS and start exploring SQL CLR.  Lets get started'
layout: post
permalink: /archives/getting-started-with-sql-clr-part-1/
dsq_thread_id:
  - 891811679
categories:
  - Databases
  - SQL Server
---
# 

Start: Anyone who has done scripting tasks in SSIS knows how valuable being able to leverage .NET can be when you are building data solutions. Based on a recent presentation, by [Tim Mitchell][1], I was encouraged to extend .NET past SSIS and start exploring SQL CLR. Let's get started:

 [1]: http://www.timmitchell.net/

First, we need to enable CLR on our database:

    sp_configure 'clr enabled', 1
    GO
    RECONFIGURE
    GO
    

Next, we fire up Visual Studio and create a Database project. C# is my language of choice so I choose it:  
![CreateProject][2]

 [2]: http://images.datachomp.com/sqlserver/sqlclr/CreateProject.jpg

After that, it will likely ask you for some DB credentials, much like creating a datasource when you are doing other .net applications. Like other .NET solutions, you can right click on your project, go to properties and change your connection on the 'database' tab. Once you get your project up, right click on add a new item, and select stored procedure. Give it a name bearing in mind that the name you give it will be what the procedure gets deployed as. Once created you will see that VS takes care of a lot of the plumbing for you:

    1.  &nbsp;
    
    2.  using System;
    
    3.  using System.Data;
    
    4.  using System.Data.SqlClient;
    
    5.  using System.Data.SqlTypes;
    
    6.  using Microsoft.SqlServer.Server;
    
    7.  &nbsp;
    
    8.  &nbsp;
    
    9.  public partial class StoredProcedures
    
    10. &#123;
    
    11. 	&#91;Microsoft.SqlServer.Server.SqlProcedure&#93;
    
    12. 	public static void clr_Chomp&#40;&#41;
    
    13. 	&#123;
    
    14. 		// Put your code here
    
    15. 	&#125;
    
    16. &#125;;
    
    17. &nbsp;

Let's throw in some code:

    1.  &nbsp;
    
    2.  	&#91;Microsoft.SqlServer.Server.SqlProcedure&#93;
    
    3.  	public static void clr_Chomp&#40;out string Chomps&#41;
    
    4.  	&#123;
    
    5.  		Chomps = "Chomp Chomp Chomp Chomp";
    
    6.  	&#125;
    
    7.  &nbsp;

What did we add? We basically declared an output parameter (out string chomps), and then said that variable is just going to return the text 'Chomp' X 4.  
Compile, and deploy. Gotcha - For those of us that use VS 2010 our default framework is 4.0 .... this won't jive in SQL Server so you will get a successful build, but a failed deployment. Follow the yellow brick errors and change your target framework down to 3.5 (if deploying to SQL Server 2008). Once that is done, it is time to see this guy in action!!!  
Make sure you are in the proper database and run it:

    1.  &nbsp;
    
    2.  DECLARE @words VARCHAR&#40;25&#41;
    
    3.  EXEC dbo.clr_Chomp @Chomps = @words OUTPUT
    
    4.  SELECT @words
    
    5.  &nbsp;

and it should spit out some Chomps for you. If it worked, go reward yourself with some M&M's. If it didn't work for you then:

    1.  &nbsp;
    
    2.  GOTO START;
    
    3.  &nbsp;

In Part 2, I will begin leaving the playground of basic examples and start showing real practical implementations of SQLCLR.  
In Part 3, I will likely look at performance and some other items to help round out the series.