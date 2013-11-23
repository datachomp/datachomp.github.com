---
layout: post
title: "Sequel and Postgres Range Types"
date: 2013-11-15 21:20
comments: true
categories: 
---
Like many rubyists, I have a hard time keeping up with which burrito truck is parked outside my house on any given day. In postgres, I've solved this by entering the trucks and the duration they will be staying parked outside in my database:

	CREATE TABLE burritotrucks
	(
	  id serial primary key,
	  truckname text not null,
	  onsite daterange not null --or tsrange if busy schedule
	);

	insert into burritotrucks (truckname, onsite)
	values ('thumpy tortillas', '[2013-11-15, 2013-11-17]'),
	('chompy delight', '[2013-11-15, 2013-11-17]'),
	('no dogs allowed', '[2013-11-16, 2013-11-21]'),
	('government cheese wagon', '[2013-11-11, 2013-11-14]'),
	('tortuga mochilla', '[2013-11-13, 2013-11-18]');

There are a variety of operators to query [range types][1], but for this example, I'll just be using the [contains operator][3]:
[1]: http://www.postgresql.org/docs/current/static/rangetypes.html
[3]: http://www.postgresql.org/docs/current/static/functions-range.html

	select * from burritotrucks where onsite @> now()
Uh oh, it's not working. Unlike the other range types, date ranges require a little more finesse. Postgres doesn't quite trust the implicit conversion so we'll just do a little hand holding:

	select * from burritotrucks where onsite @> now()::date
	
It works! But how do we do this in our application with [Sequel][2]? First, we let [Sequel][2] know we're going to be using the range extensions:

	Sequel.extension(:pg_range)
	DB = Sequel.connect(:adapter=>'postgres', :host=> 'localhost'
	, :database=>'frontyard', :user=>'dc')

We query the database:

	options = DB[:burritotrucks].all
	p options
and we get the data. We can see how beautifuly [Sequel][2] handles the range type for us in the printed output and at this point we can get away with using our raw sql to get the data we need:

	options = DB.fetch("select * from burritotrucks where onsite @> now()::date").all
	p options
This works, but I can imagine many AppDevs becoming ill at the sight of SQL in their app. In order to appease both sides of the aisle, [Sequel][2] also has some pretty ruby friendly operators we can use by just adding the core_extensions as well as the range operators extension:

	Sequel.extension(:core_extensions, :pg_range, :pg_range_ops)
	DB = Sequel.connect(:adapter=>'postgres', :host=> 'localhost'
	, :database=>'frontyard', :user=>'dc')
	
Now we can get much more ruby friendly queries. Note, We still need to cast for our date value but that is pretty trivial:
	options = DB[:burritotrucks].where(:onsite.pg_range.contains(Sequel.cast(Date.today, Date))).all
	p options


People often ask me why I like [Sequel][2] so much and this is another great example why. It doesn't punish me for knowing SQL. It doesn't punish postgres for having so many amazing features and data types. It easily lets me know how many burrito trucks I have in my yard which is something you can not put a value on.

[2]: https://github.com/jeremyevans/sequel