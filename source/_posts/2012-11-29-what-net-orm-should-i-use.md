---
title: What .NET ORM should I use?
author: Rob
layout: post
permalink: /archives/what-net-orm-should-i-use/
dsq_thread_id:
  - 950760729
categories:
  - AppDev
  - Databases
---
# 

I get asked all the time "What ORM should I use?" I typically answer the person with some spaghetti types/talks which of course runs counter to the DRY principle. To help get away from that, I'll hash out some of my typical responses below. One thing to keep in mind when reading, is that all of the ORMs mentioned below are really good choices. I'm "picking" one per situation as a way of helping someone avoid the paradox of choice. 

**"Should I hand-roll my DAL?"**

No. Unless you just want an academic/code exercise. The libraries I'll mention in this post are already built, tested, fast and easy to use. If you are serious about building your application, don't waste time reinventing this wheel.

  


**"I don't fear dynamics and I want something easy or rapid prototyping"**

[Massive][1] - This ORM is simple to use, completely gets out of my way and lets me get right to building.

  


**"I don't fear dynamics and I want some codefirst action"**

[Oak][2] - One of my coding heroes [Amir Rajan][3] has gone and taken everything good about ActiveRecord and melded it with Massive.

  


**"I want POCOs, an old school feel and Stored Procs"**

[Dapper][4] - When you need uncompromised speed and ease, Dapper is your tool. The Stackoverflow team uses it and that is one heck of a merit badge.

  


**"I want POCOs with some codefirst action"**

[OrmLite][5] - ORMLite is a Post-CRUD augmentation to Dapper. In some ways, it is to Dapper what Oak is to Massive. Coded in awesome, covered in love.

  


**"I'm using [ServiceStack][6] or planning to"**

[OrmLite][5] - This one is kind of obvious as OrmLite is part of the [ServiceStack][6] family.

  


**"I am a huge fan of T4 and code gen"**

[PetaPoco][7] - PetaPoco is pretty fun regardless of your needs. If you are already pretty invested in T4 then this is for sure a love connection.

  


**"I want POCOs and I might be doing some MongoDB in my app"**

[Simple.Data][8] - I have personally not used this one yet, but I always hear wonderful things about it.

  


**"My boss says we just have to use something ~Enterprisey~"**

Use OrmLite and tell them it is a MS ADO.NET CPT Gen2 Release of EntityFramework with Enhanced Cloud Coverage™ and Synergized Mix-ins™.

  
  


 [1]: https://github.com/robconery/massive "Massive"
 [2]: https://github.com/amirrajan/Oak "Oak"
 [3]: http://www.amirrajan.net/ "Amir"
 [4]: http://code.google.com/p/dapper-dot-net/ "Dapper"
 [5]: https://github.com/ServiceStack/ServiceStack.OrmLite "OrmLite"
 [6]: http://servicestack.net/ "ServiceStack"
 [7]: http://www.toptensoftware.com/petapoco/ "PetaPoco"
 [8]: https://github.com/markrendle/Simple.Data "Simple.Data"

While all these ORMs are pretty easy to use, you should always be doing yourself/your app (and of course your DBA) a favor and profile your DAL. I highly recommend [ORMProfiler][9]. I think it supports most if not all of the above ORMs, has a low cost, and you get a ton of information from it.

 [9]: http://www.ormprofiler.com/ "ORMProfiler"