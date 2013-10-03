---
layout: post
title: "Postgres.app and shared_preload_libraries"
date: 2013-09-18 09:28
comments: true
categories: 
---
Modules and extensions are one of the awesome features that keep Postgres so relevant in modern development. Need GUIDs? Just add the extension and you're off to the races:
	select * from pg_available_extensions where name = 'uuid-ossp'; -- check for uuid-ossp
	create extension "uuid-ossp"; -- it's alive!
and to make sure it's active:
	select * from pg_extension; -- we should see it listed

But not all extensions are created equal. If you live in DBA world, one of the extensions you come to lean on is the [pg_stat_statements][2] extenion. This extension gives you  deeper view of what has been running in your database. Adding it to a database is easy enough:
	create extension "pg_stat_statements";
  	select * from pg_stat_statements;  

Uh oh, we're greeted with a error that can be a little intimidating:  
<font color=red>ERROR:  pg_stat_statements must be loaded via shared_preload_libraries</font>  

Ummmm what? Some extensions have special needs that require us to alter our configuration for our entire Postgres instance. pg_stat_statements has some additional memory requirements that need to be satisfied on startup thus requiring us to add it to the shared_preload_libraries. If we're on Ubuntu, we make sure we have our contrib module installed:
	sudo apt-get install -y postgresql-contrib-9.3

Head on over to our configuration at /etc/postgresql/9.3/main/postgres.conf and look for the shared_preload_libraries setting and reference the extension:
	shared_preload_libraries = 'pg_stat_statements' # (change requires restart)

This works great for us trench diggers on Linux, but what about the AppDev's that can't be separated from their Mac's? If you are on a Mac, I sincerely hope you're using the awesome [Postgres.App][1], but finding the config for it isn't straight forward. Luckily, Postgres has our back. Connect to your instance of Postgres.app and run the following:
	show all;        -- This is a list of a bunch of settings... whoa!
	show config_all; -- Ahhhh, here is what we want

Now that we have the location of our configuration file, just throw it into the Unholy Explorer(AKA Finder), start scrolling the file and add the pg_stat_statements setting to the shared_preload_libraries.

#### buyer beware
This works wonderfully in Postgres.app 9.3.0.0. In earlier versions of the app, it does has issues finding the contrib modules. This is a bit of a bummer and when I get my act together, I'll file an issue. Of course, you're more than welcome to beat me to the punch.


[1]: http://postgresapp.com/
[2]: http://www.postgresql.org/docs/current/static/pgstatstatements.html