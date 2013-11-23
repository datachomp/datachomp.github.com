---
layout: post
title: "heroku pg-extras builds bridges"
date: 2013-11-20 19:10
comments: true
categories: 
---
It starts with the all too familiar "I think my database is running slow" and ends with the AppDev and myself speaking different languages, yelling at each and saying hurtful things... all the while the app and db sit in their servers neglected and needing love.

### pg_therapy
While many of us that live in the database layer have tricked out [.psqlrc][1] files, I rarely find AppDevs with the same. Let me stress that this is perfectly ok!!! But having some really ugly diagnostic queries aliased in your .psqlrc is incredibly helpful. Majority of the AppDevs I talk to concerned about their db also happen to be running on Heroku. To accomodate its users, Heroku has put out a very helpful plugin to their toolbelt called [pg-extras][2]. I love this tool because it gets the AppDev and I on a common language. They don't get scared by hairy SQL statement and it's consistent from app to app. Check it out:

### Pump you up
Installation is a breeze:
	heroku plugins:install git://github.com/heroku/heroku-pg-extras.git

Using it a breeze:
	heroku pg:bloat  #one of my favorites!
When I run that, it tells me that I didn't specify a database:  
! Unknown database. Valid options are: HEROKU_POSTGRESQL_BLACK_URL, HEROKU_POSTGRESQL_PINK_URL  
  
I run it again with the proper db:
	heroku pg:bloat HEROKU_POSTGRESQL_BLACK_URL
Now I have an easy to use snapshot of our database that has an incredibly low barrier of entry to execute for myself or anyone. Let's say that the person running it doesn't trust me or perhaps I'm in a burrito coma, they can always refer to the [heroku database tuning site][3] and get sound information.

### Dude, I don't even have time for that.
Maybe the above is a bit too much to stomach. That's cool, we'll just flip another easy switch:
	heroku addons:add librato
[Librato][4] is a beautiful dashboard that is fun to look at, easy to use and will give us some historical context for troubleshooting the typically ill fated "I think my database is slow" situation. What is nice about tooling like Librato is not only does it show our db stats, we also get the context of the Application(Dyno's, router latency, etc). For more information on this, checkout Craig Kerstiens post on [Monitoring with Librato][5].

### Hugs and Queries
I love this type of solution because it gets us looking at hard numbers and can easily as well as objectively say it is the DBs fault, the Apps fault, heroku's fault, or no ones fault and everything is fine. Librato and Postgres don't keep track of our egos, our mood or our fatigue - they keep us honest and on topics that really matter... something we all could use a little more of.


[1]: https://github.com/datachomp/dotfiles/blob/master/.psqlrc
[2]: https://github.com/heroku/heroku-pg-extras
[3]: https://devcenter.heroku.com/articles/heroku-postgres-database-tuning
[4]: http://librato.com
[5]: https://postgres.heroku.com/blog/past/2013/10/9/monitoring_your_heroku_postgres_database/