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

    1.  &nbsp;
    
    2.  var tbl = [new][2] Products&#40;&#41;;
    
    3.  var products = tbl.All&#40;where: "CategoryID = @0 AND UnitPrice > @1", orderBy: "ProductName", limit: 20, args: 5,20&#41;;

 [2]: http://www.google.com/search?q=new msdn.microsoft.com

It makes the args appear to be this completely bad ass and intelligent command that knows exactly when and where to parse/apply the values. Well, in the real world versions of Massive the compiler doesn't take to kindly to that sort of syntax.

args: is a parameter of type object [] so the way I use multiple params is as follows:

    1.  &nbsp;
    
    2.  object&#91;&#93; queryargs = &#123;"Okapi", "online"&#125;;
    
    3.  var DBBasics = table.All&#40;where: "WHERE ServerName=@0 and status = @1", args: queryargs &#41;;
    
    4.  &nbsp;

and low and behold the magic happens.