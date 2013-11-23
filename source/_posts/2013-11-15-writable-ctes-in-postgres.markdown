---
layout: post
title: "Writable CTEs in Postgres"
date: 2013-11-15 18:10
comments: true
categories: 
---
Postgres is filled to the brim with awesome features, but they don't make sense for every occasion. I posted last night about my [Thankyou app][1] and it happens to have a use case for writable [CTEs (Common Table Expression)][2].  
[1]: http://datachomp.com/archives/my-friend-rob/
[2]: http://www.postgresql.org/docs/current/static/queries-with.html

  
First, lets new up some data:  
	drop table if exists thanks;
	create table thanks (
		id serial primary key,
		who text not null,
		picked boolean not null default 'false',
		created_at date default now(),
		last_picked date default '-infinity'
	);

	insert into thanks (who)
	values ('rob conery'), ('postgres'), ('sidekiq'), ('demis bellot'), ('sinatra')
	, ('josh berkus'), ('elizabeth naramore'), ('amir rajan'), ('sequel');
		
	select * from thanks;

Boom! This app selects a random person or project I'm thankful for that hasn't already been picked or hasn't been picked in the last 9 months. Here it is in code form:

	select * from thanks where picked = false or last_picked < now() - interval '9 months';

Now we can put it in a normal CTE:
	with guesswho as (
		select * from thanks 
		where picked = false or last_picked < now() - interval '9 months')
	select guesswho.id, guesswho.who
	from guesswho;

Yes!!! Let's pick a random row. For our randomizer, I would like a lovely set of sequential id's I can pick from. Since we can't completely trust the primary key id's returned in our CTE (especially as rows start to get trimmed off), we're going to throw a row_number function on, as well as pass our first CTE into a second CTE to generate a random number based on the result set:

	with guesswho as (
		select ROW_NUMBER() OVER (ORDER BY id) as champs, * 
		from thanks where picked = false or last_picked < now() - interval '9 months')
	, onlyone as (select trunc(random() * count(0) + 1) as tops from guesswho)
	select guesswho.id, guesswho.who
	from guesswho, onlyone
	where champs = onlyone.tops;

Yay!!! We're getting data we love on projects we love. But where does the writable CTE come into play? How cool would it be if we could also mark the record we're selecting as picked, so we don't have to make an additional call to the DB to flag afterwards? Check it out:
	with guesswho as (select row_number() over (order by id) as champs, * 
		from thanks where picked = false or last_picked < now() - interval '9 months')
	, onlyone as (select trunc(random() * count(0) + 1) as tops from guesswho)

	, adios as (update thanks set picked = 'true', last_picked = now() from guesswho, onlyone where thanks.id = guesswho.id and 
	guesswho.champs = onlyone.tops RETURNING thanks.picked)
	
	select guesswho.id, guesswho.who
	from guesswho, onlyone, adios
	where champs = onlyone.tops;

So.Much.Awesome! Check out that 3rd CTE we added (adios), is using our previous CTE's, updating the row we chose, as well as providing a returning statement of what it updated. The returning statement provides context for what happened inside that writable CTE to alleviate confusion between table expresisons. Feel free to play with it on your own machine and especially change the data for the people or tech you're thankful for.