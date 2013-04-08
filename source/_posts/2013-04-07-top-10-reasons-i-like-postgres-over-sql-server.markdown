---
layout: post
title: "Top 10 Reasons I like Postgres Over SQL Server"
date: 2013-04-07 10:46
comments: true
categories: 
---
{% img left http://static.datachomp.com/robandguillaume.jpg Rob and Guillaume %}
While at [Waza][1] this year, I had a chance to talk to my sfriend [Guillaume Roques][2]. In addition to talking about SalesForce, we took advantage of our mutual .NET backgrounds to discuss Microsoft. We did the typically uncompromising praise of [The Gu][3] and how far Azure has come along in the last 18 months... and of course we had to talk databases. Below is my quick little list of reasons I gave him as to why I'm favoring Postgres over SQL Server from a technical/business aspect.
  
<br />
  
<hr />
1. SQL Server still to this day deploys pessimistic concurrency out of the box. Anyone not aware of this "feature" starts out very disadvantaged on performance. This person will soon get an internet history filled with locking/blocking/deadlocking links. Postgres defaults to optimistic concurrency via it's MVCC feature and is a joy to work with.

2. Compression out of the box. With SQL Server, compression is an "Enterprise Edition and Up" feature which means you are spending the cost of at least 1 dev in order to get the ability to use compression. Once you have paid for that ability, you still have to figure out how to implement it. Postgres does this for you out of the box, automatically and for free.

3. Concurrent Index Creation. You are going to find a recurring theme there...this is yet another feature that SQL Server is capable of doing, but only if you are able to afford the elite and affluent company of Enterprise Edition. Postgres has your back on this even if you left your wallet at home.

4. Partitioning out of the box. Our data is growing and we need to do something about it, but perhaps we don't want to take on the drama of sharding yet. Partitioning can be a pretty good fix for this and with Postgres, unsurprisingly, you get it out of the box. As for SQL Server, basic table partitioning is only available in Enterprise Elite edition. Yep, you're writing huge checks to pull this off.

5. Indexable functions - In Postgres, you can actually index certain functions and maintain sargability.  With SQL Server, you're stuck in the cruel world of table scans when this happens.

6. Modules/Extensions - Tech moves fast. The fact that Postgres has an extensions systems means that your DB platform can now match the innovations on your web stack. SQL Server has releases every 3 years... It quickly becomes your grandparents' database.

7. Works everywhere - Linux, Mac, BSD, Windows... Postgres comes to you on the platform you want to code on. SQL Server is flexible in that it works on all 50 versions of Windows Vista, Windows 7, Windows 8. 

8. Arrays - Everyone loves arrays and they are a core part of programming...except in SQL Server, where they don't exist.

9. JSON / V8 support - You can write/use JSON and Javascript on the client, server and even your database if you are using Postgres. SQL Server? I believe they are still fully invested in thinking that XML is the future.

10. Unicode by default - No longer do you have to play the nvarchar / varchar game or silly collation battles like you do in SQL Server. Postgres is UTF-8 out of the box and wants nothing more than to give your app gigantic, unconditional data hugs.

[1]: http://waza.heroku.com/2013/
[2]: http://twitter.com/groques/
[3]: http://en.wikipedia.org/wiki/Scott_Guthrie