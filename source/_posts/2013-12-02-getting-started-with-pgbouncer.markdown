---
layout: post
title: "Getting Started With PGBouncer"
date: 2013-12-02 22:00
comments: true
categories: 
---
One of the issues we face at Raisemore is a continuous flux in our number of connections to our primary Postgres database. If our load increases and we have to spin up more front end servers or spin up more backend servers. This can quickly wreck our connection limit. One solution, is for me to babysit the connection size on the pg server itself... or perhaps I can give the Apps the middle finger and starve their connection limit - neither of these lead to happy outcomes for everyone involved.

###apt-get install happiness
A better compromise is to just put a connection pooler like [PgBouncer][1] in front of our postgres database. In the same way we proxy our applications with Nginx, we can apply the same concept to our database(s) with Pgbouncer. Let's get started:
	--Assuming your pg database is already up and running
	--databasename = burrito_hq  username = elguapo
	sudo apt-get install -y pgbouncer
	sudo nano /etc/pgbouncer/userlist.txt
	> "postgres" "postgres"
	> "elguapo" "hefe"
	sudo nano /etc/pgbouncer/pgbouncer.ini
	> [databases]
	> burrito_hq = host=localhost port=5432 dbname=burrito_hq user=elguapo password=hefe
	> [pgbouncer]
	> listen_addr = *
	> auth_type = md5
	> admin_users = postgres
	> stats_users = postgres

Above are some of the settings I changed to get started. As you get more familiar with Pgbouncer, change the [various settings][3] as you see fit and for your workload.  
Let's test our settings by firing up PgBouncer. PgBouncer is closely tied to the postgres user on the server, so we'll 'switch user' into the postgres account to crank it up:
	sudo su postgres -c"pgbouncer -d /etc/pgbouncer/pgbouncer.ini"
On our client machine, lets attempt a connection to our server which has an ip of 172.16.150.128:
	psql -h 172.16.150.128 -p 6432 -U elguapo -d burrito_hq 
and hopefully we're in!  Once we know our settings are in good shape, lets turn PgBouncer auto start mode on:
	sudo nano /etc/default/pgbouncer
	> START=1
	sudo reboot #reboot the box to test it out
	
When the server comes back online, hopefully you can connect again from the client. Now that the server is stable again, we can go get completely lost in the [documentation][2] and let the tool really shine.


[1]: http://wiki.postgresql.org/wiki/PgBouncer
[2]: http://pgbouncer.projects.pgfoundry.org/doc/usage.html
[3]: http://pgbouncer.projects.pgfoundry.org/doc/config.html