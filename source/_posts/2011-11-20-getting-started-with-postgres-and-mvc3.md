---
title: Getting started with Postgres and MVC3
author: Rob
excerpt: |
  "<a href="http://www.postgresql.org/">Postgres</a>? WTF man, you're a SQL Server DBA!"  It's true, I am a SQL Server DBA but even I have to admit that the Postgres team is doing some wildy exciting stuff.  With that in mind, lets take a quick look at just how quickly we can replace SQL Server with a free/open full bore Enterprise database system like <a href="http://www.postgresql.org/">Postgres</a>.
layout: post
permalink: /archives/getting-started-with-postgres-and-mvc3/
dsq_thread_id:
  - 888405019
categories:
  - MVC
  - Postgres
---
# 

"***[Postgres][1]? WTF man, you're a SQL Server DBA!***" It's true, I am a SQL Server DBA but even I have to admit that the Postgres team is doing some [wildy exciting stuff][2]. With that in mind, lets take a quick look at just how quickly we can replace SQL Server with a free/open full bore Enterprise database system like [Postgres][1].

 [1]: http://www.postgresql.org/
 [2]: http://www.postgresql.org/about/featurematrix

One of the first things you will notice is Postgres is way easier to install than SQL Server. The SQL Server install is born from a truck stop romance between TFS and Sharepoint that someone found in a garbage bag in a dumpster and burned to a DVD. Postgres takes a different angle and opts for a 'less is more' type of approach. One of the big things you will want to pay attention to is the password you give the default 'postgres' account. Think of him as the SA account in SQL Server as he too wields a mighty big stick.

Once we finish the installer, lets open up pgAdmin III (henceforth known as pgAdmin). If you are accustomed to SSMS with SQL Server and always wished it went on a diet... then you might like pgAdmin. Be warned though, pgAdmin isn't SSMS on a diet... it is like SSMS with Anorexia. Once pgAdmin is opened and you are connected, then drill down to 'databases' and do a 'new database' and use the GUI to make your fancy new demo database, or just run the raw sql:

    1.  &nbsp;
    
    2.  CREATE DATABASE "Demo"
    
    3.    WITH ENCODING='UTF8'
    
    4.         OWNER=postgres
    
    5.         CONNECTION LIMIT=-1;
    
    6.  &nbsp;

Next, lets hop in that database and throw up some tables/data:

    1.  &nbsp;
    
    2.  CREATE TABLE "Colors"
    
    3.  &#40;
    
    4.  	colorid serial NOT NULL,
    
    5.  	colorname character varying&#40;50&#41; NOT NULL,
    
    6.  	constraint pk_colorid PRIMARY KEY &#40;colorid&#41;
    
    7.  &#41;
    
    8.  WITH &#40;OIDS=false&#41;;
    
    9.  &nbsp;
    
    10. INSERT INTO "Colors" &#40;colorname&#41;
    
    11. VALUES &#40;'red'&#41;;
    
    12. INSERT INTO "Colors" &#40;colorname&#41;
    
    13. VALUES &#40;'blue'&#41;;
    
    14. SELECT * FROM "Colors";
    
    15. &nbsp;

So why the quotes around the table names you might be asking... well, SQL Server has a bit of Honey Badger approach to what case you assign names and Postgres treats it like a pretty big deal. Luckily, overcoming it isn't a big deal, we just double quote it. Now, if you don't have years of tsql or [camelcasing][3] drilled into your brain, you can easy just do everything in lower case and not fight the double quote dragon.  
What else sticks out there... oh Data Types! You will have to adjust to some variance in data types, but in a good way. Postgres has a lavish assortment of data types for you to fall in love with and one of which is using 'serial' instead of the whole IDENTITY(1,1) you might be used to in SQL Server. There are also some really cool things called Sequences in Postgres which are blazing fast and give you the value of the ID before you insert it. DBAs like them because it can get you AppDevs away from fatty GUID primary keys.

 [3]: http://en.wikipedia.org/wiki/CamelCase

Fire up a new MVC3 app and hop into the Nuget console. From there, we're going to add our [Postgre provider][4] with "**Install-Package Npgsql**". Now that we have our vital provider assemblies, lets go mess around in the web.config.

 [4]: http://www.nuget.org/List/Packages/Npgsql

    1.  &nbsp;
    
    2.  
    
    3.  	
    
    4.  		
    
    5.  			
    
    10. 		
    
    11. 	
    
    12. &nbsp;
    
    13. 	
    
    14. 		
    
    15. 	
    
    16. &nbsp;

Yeah! We have our provider factory rocking and our connection string. Yes, I am using the postgres account in this demo and yes that is wrong/stupid, but I'm having so much fun playing with the Postgres engine that I decided to take a shortcut there for this entry.

So our color app is pretty quick and dirty. In keeping with that spirit, I'm going to use [Massive][5] as my ORM of choice. It already has a [Postgres version][6] ready to go. [PetaPoco][7] has some very nice Postgres options in it, but Massive is very quick/easy and the Expandos makes all the strong-typers uncomfortable and I love a good code-trolling.

 [5]: https://github.com/robconery/massive
 [6]: https://github.com/robconery/massive/blob/master/Massive.PostgreSQL.cs
 [7]: https://github.com/toptensoftware/petapoco

In the Model folder of our app, I create a new class file called Massive and I copy/paste the [Postgres version][6] into it, and I also created a new class file called "Colors" that I pasted the following in:

    1.  &nbsp;
    
    2.  	public class Color : DynamicModel
    
    3.  	&#123;
    
    4.  		public Color&#40;&#41; : base&#40;"pg", "Colors", "colorid"&#41; &#123; &#125;
    
    5.  &nbsp;
    
    6.  	&#125;
    
    7.  &nbsp;

All wired up! In the home controller, we'll throw some code to play with our table in:

    1.  &nbsp;
    
    2.  var table = [new][8] Color&#40;&#41;;
    
    3.  var colors = table.All&#40;&#41;;
    
    4.  ViewBag.colors = colors;
    
    5.  &nbsp;

 [8]: http://www.google.com/search?q=new msdn.microsoft.com

And then in our view:

    1.  &nbsp;
    
    2.  @foreach &#40;var color in ViewBag.colors&#41;
    
    3.  &#123; 
    
    4.  	 @: @color.colorid @color.colorname
    
    5.  &#125;
    
    6.  &nbsp;

F5 and what do we get? BOOM!!! "Error 42P01: relation "colors" does not exist"  
So remember when I started our Colors table with a capital C? This is where it can come back to bite you... and also one of the reasons I absolutely love Micro-ORMs. Fast, malleable, and so easy a DBA can use it. Let's hop into Massive and throw some quotes around our table access:

    1.  &nbsp;
    
    2.  string sql = limit >  ? "SELECT TOP "   limit   " {0} FROM "&#123;1&#125;" " : "SELECT {0} FROM "&#123;1&#125;" ";
    
    3.  &nbsp;

and Viola! It works! In pretty much the time it takes to install SQL Server, we're already rocking our stunning and highly valuable colors app!