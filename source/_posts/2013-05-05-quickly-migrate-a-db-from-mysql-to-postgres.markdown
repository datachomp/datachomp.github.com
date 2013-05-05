---
layout: post
title: "Quickly Migrate A DB From MySQL To Postgres"
date: 2013-05-05 12:50
comments: true
categories: 
---

The more I'm around people building apps in Ruby or ruining their quality of life with PHP, the more I'm seeing them actually use MySQL for something other than punch lines. As someone dedicated to trying to make the world a better place, I began looking for some easy ways to migrate from MySQL to [Postgres][2]. This post will will not even pretend to be an exhaustive list for how to do this, but more based around me having 15 minutes of free time and some internet.

The first place I went looking was [Railscasts][1] - They have a free intro episode to [Postgres][2] and one of the tools they show is [Taps][3].  I gem'ed taps real quick and gave it a shot, but I couldn't get it to run due to port binding dependencies on my machine. No worries though, I still had plenty of tabs with search results so I moved on.

[Valkyrie][4] was the next option I tried. I gem'ed it up, pointed it at the DB's and then it barfed on a 'mysql' dependency (not to be confused with mysql2 gem dependency). I installed the 'mysql' gem, ran it again and voilà! Migration complete! The command issued was very simple:
> valkyrie mysql://datachomp@localhost/seppuku?password=QuickAndPainless postgres://datachomp@127.0.0.1/seppuku

A couple things I like about Valkyrie:

- It is simple. I just provide a mysql source and postgres destination.
- It uses [Sequel][7].
- I can keep re-running it without it duplicating source data.


If you are needing something more hardcore than a 1 time migration, the ever resourceful [Craig Kerstiens][5] suggested I check out [Tungsten-Replicator][6]. Tungsten will essentially set up a read slave from MySQL to Postgres. Obviously, setup/configuration is going to be a bit more involved with Tungsten than a ~Fire-and-Forget~ migration tool like Valkyrie but remember, you are doing it for a good cause.

Now that you have seen an easy way to migrate away from MySQL, I encourage you to scan your network and start making the world a better place.

[1]: http://railscasts.com/episodes/342-migrating-to-postgresql
[2]: http://www.postgresql.org/
[3]: https://github.com/ricardochimal/taps
[4]: https://github.com/ddollar/valkyrie
[5]: http://www.craigkerstiens.com/
[6]: https://code.google.com/p/tungsten-replicator/
[7]: https://github.com/jeremyevans/sequel