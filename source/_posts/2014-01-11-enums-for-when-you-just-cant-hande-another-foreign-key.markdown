---
layout: post
title: "Enums for when you just cant hande another foreign key"
date: 2014-01-11 10:11
comments: true
categories: 
---

If we get over the notion that all data is created equal, it opens up options for us. 
For example, your DBA wants some sort of data consistency and integrity... both very good things
but you are just making a quick little 1 off table that you don't want to sprawl to 4 or 5 tables with foreign keys.

#### enum my heart
For these situations, I like to just use [postgres enums][enum]. Check it out:

	create type verdict as enum ('gross', 'can eat', 'amazing');

	create table burritos (id serial not null primary key, title varchar(50), thoughts verdict not null);
	insert into burritos (title, thoughts) values ('road kill burrito', 'yummy');

	insert into burritos (title, thoughts) values ('road kill burrito', 'gross');
	--Complete Failure

	insert into burritos (title, thoughts) values ('grande queso burrito', 'amazing');
	--Complete Awesome
Sweet! We're getting the data we want in, it looks legit, and there is no querying of multiple tables to get the values back.
Another benefit to this is that AppDev isn't continually questioning if they set up the FK correctly.

#### blowing in the winds of change
But how maintainble is it? Say we had a less than ideal experience... one that requires a lawyer.  need to add to our list:

	alter type verdict add value 'lawsuit' before 'gross';
	insert into burritos (title, thoughts) values ('putrid plate', 'lawsuit');

That was pretty easy, though I must say I'm a little confused on exactly which values I have in my enum. Good thing we're in a database and can just query it!!!:  

	select t.typname as enum_name,  
	       e.enumlabel as enum_value,
		   n.nspname as enum_schema
	from pg_type t 
	   inner join pg_enum e on t.oid = e.enumtypid  
	   inner join pg_catalog.pg_namespace n ON n.oid = t.typnamespace
	where t.typname = 'verdict' and n.nspname = 'public'

#### re-fear-factor
If you are ok with the above code... then good on you. However, we might want to populate a dropdown or something else in our application based on these values. Neither your typical Rails Dev or ActiveRecord itself is going to try to implement the above without tears of saddness streaming down to their Mac. We can build a bridge by just making a simple view and assigning a self.table_name = to the following:

	create view my_lovely_enums as 
	select t.typname as enum_name,  
	       e.enumlabel as enum_value,
		   n.nspname as enum_schema 
	from pg_type t 
		inner join pg_enum e on t.oid = e.enumtypid  
		inner join pg_catalog.pg_namespace n ON n.oid = t.typnamespace
	where t.typname = 'verdict' and n.nspname = 'public';

Hardcoding 'verdict' like that creates a pretty brittle where clause. I tend to remove that part and go a little more flexible with a predicate/scope to select your enum of choice:

	create or replace view my_lovely_enums as 
	select t.typname as enum_name,  
	       e.enumlabel as enum_value,
		   n.nspname as enum_schema
	from pg_type t 
		inner join pg_enum e on t.oid = e.enumtypid  
		inner join pg_catalog.pg_namespace n ON n.oid = t.typnamespace;
	select * from my_lovely_enums where enum_name = 'verdict' and n.nspname = 'public';
	"verdict";"gross";"public"
	"verdict";"can eat";"public"
	"verdict";"lawsuit";"public"
	"verdict";"burrito dreams";"public"

For some more information on this topic, check out a lovely write up at [postgresguide.com][craig]. The bottom of that article has this handy little  buyer-beware:  
"At the time of this writing, Postgres does not provide a way to remove values from enums."
While this approach can't be used for all situations, using it where you can get creates a tremendous amount of simplicity in your app without compromising integrity - affectionally known as a win-win.


[enum]: http://www.postgresql.org/docs/current/static/datatype-enum.html
[craig]: http://postgresguide.com/sexy/enums.html