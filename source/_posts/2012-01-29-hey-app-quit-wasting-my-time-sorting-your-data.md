---
title: Hey App, quit wasting my time sorting your data
author: Rob
layout: post
permalink: /archives/hey-app-quit-wasting-my-time-sorting-your-data/
dsq_thread_id:
  - 888405052
categories:
  - Databases
---
# 

An issue I run into quite a bit is unnecessary sorting in the database. I'm not talking about the sort of 'Get last 5' type of sorting where you need to sort to get a valid result set. I'm talking about the '*Hey Database! I want some data...and I'll probably throw some business logic in you... and while I'm here, how about we throw the presentation layer in as well and you sort the results for our UI!*'

**Nut Kicking:**  
In the same way that AppDevs outnumber DBAs, infrastructure wise there are typically way more web/caching servers than there are database servers. This is mostly due to the fact that like a decent DBA, a decent database server is expensive. AppDevsWebservers in general are cheaper, have less memory and don't need to be as awesome as a DBAdatabase server.

**Code Please:**  
Lets take a look at some execution plans/cost so you can view 'ORDER BY' the same way I do.  
Below is a simple example of selecting some badges by userid, and then display them alphabetically for the user to view :  
<code>SELECT  Name  
FROM dbo.Badges  
WHERE userid = 91254;  
SELECT Name  
FROM dbo.Badges  
WHERE userid = 91254  
ORDER BY name DESC;</code>
And this is the execution plan it creates:  
[![execplan][2]][2]

 []: http://files.datachomp.com/AppDev/orderby/orderbyexecutionplan.png
 [2]: http://files.datachomp.com/AppDev/orderby/orderbyexecutionplan.png

On the bottom, do you see the glyph that is "Sort Cost: 15%"? As well as a difference of almost 10% in general between the two queries? Removing those "sort"(har har har) of thing adds up...like a lot.

**Just Fix It**  
In C#, you have these things you can use called Ordered Enumerables and they are really easy to use... take a look:

<code>
public IOrderedEnumerable<dynamic> GetBadgeByUserId(int badgeid)
{
	var table = new Badges();
	var badges = table.query("SELECT  Name FROM dbo.Badges WHERE userid = @0", args: badgeid);
	return badges.OrderBy(x=>x.Name);
	//return badges.OrderByDescending(x => x.Name);
}
</code>
 [3]: http://www.google.com/search?q=new msdn.microsoft.com

That wasn't too hard was it? In the above example, it is making a call to the DB (using [Massive][4]) and sucking the results into 'badges'. That is where it breaks off its relationship with the database, and sorts the results in 'badges' and returns them to whatever was calling it.

 [4]: https://github.com/robconery/massive

**Does this make us happy?**  
I'm happy because you're not putting extra load on the DB. You're happy because you have some sorted data and can close a help ticket... everyone wins right?

**But But But, our servers are busy too!**  
Ahhh, but perhaps you're one of those clever AppDevs who says that if the DB is getting over worked, then the webservers are too! Since I don't have the webserver metrics, I can't really object to that. But what I can say is: "*Yo, that's cool. Since you are a programmer.... program up some javascript to sort the results in the UI. Then, the webservers and the db can both go listen to dub step or whatever servers like to do in their spare time.*"

There are a ton of ways to do this:  
[http://lmgtfy.com/?q=sort+a+table+with+jquery][5]

  [5]: http://lmgtfy.com/?q=sort+a+table+with+jquery

Doing it on the front end also works great if you are stuck using some particular DALs or ORMs that take a bulimic approach to data retrieval IE - Eating everything in site and then barfing it out to the app.