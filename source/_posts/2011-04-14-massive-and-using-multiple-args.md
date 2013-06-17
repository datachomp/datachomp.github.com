---
title: 'Massive and using multiple args:'
author: Rob
layout: post
permalink: /archives/massive-and-using-multiple-args/
dsq_thread_id:
  - 888404993
categories:
  - AppDev
  - Databases
  - MVC
  - SQL Server
---
# 

So, in [Rob Conery's example][1] of using multiple args in a query

 [1]: http://blog.wekeroad.com/helpy-stuff/and-i-shall-call-it-massive

<code>
    var tbl = new Products();
	var products = tbl.All(where: "CategoryID = @0 AND UnitPrice > @1", orderBy: "ProductName", limit: 20, args: 5,20);
</code>
It makes the args appear to be this completely bad ass and intelligent command that knows exactly when and where to parse/apply the values. Well, in the real world versions of Massive the compiler doesn't take to kindly to that sort of syntax.

args: is a parameter of type object [] so the way I use multiple params is as follows:
<code>
	object[] queryargs = {"Okapi", "online"};
	var DBBasics = table.All(where: "WHERE ServerName=@0 and status = @1", args: queryargs );
</code>
and low and behold the magic happens.