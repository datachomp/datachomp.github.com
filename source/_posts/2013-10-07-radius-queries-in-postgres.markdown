---
layout: post
title: "Radius Queries in Postgres"
date: 2013-10-07 13:56
comments: true
categories: 
---
Today, I found myself in a horrible situation. I was heading down i-235 and I became stricken with an insatiable burrito urge. All the apps on my phone were borked and all I had was my trusty Postgres database. I needed to find a burritos shop and I needed to find one fast.

#### Where oh where to begin
Luckily, I happen to know the latitude and longitude of all my favorite burrito places in Oklahoma City. I quickly add them to my database:
	--Hello Table!
	CREATE TABLE burritoplaces
	(id serial PRIMARY KEY,
	establishment varchar(50) not null,
	lat double precision,
	lon double precision);  
	--Hello Data!
	INSERT INTO burritoplaces(establishment, lat,lon)
	VALUES ('Verde Bueno Burrito', 35.484388,-97.505035)
	,('Locos Lobos', 35.496025,-97.510185)
	,('Captain Crustacean', 35.51468,-97.524347)
	,('Double Stuffers Cafe', 35.523762,-97.508039)
	,('Cafe Truncate of Watonga', 35.866413,-98.473206);

The data checks out:
	-- all 5 entries
	SELECT * FROM burritoplaces;
Perfect! All my favorite places are there but sadly, I can't do geospatial math in my head. I can't even really do it outside of my head so I'm going to have to get some help. Enter the [earthdistance][1] postgres extension and its helpful sibling 'cube':

	--turn on the magic
	CREATE EXTENSION cube;
	CREATE EXTENSION earthdistance;

I'm becoming so weak... I need to wrap this up. Since I live in one of the four countries in the world that uses the Imperial system of measurement, I need to figure out how to do this whole thing in miles. Skimming over the earthdistance documentation, it looks like I just need to use the point commands. I whip up a query that is as tech tasty as some carne asada:

	-- my location on i-235 is a very hungry (35.512363,-97.515678)
	SELECT *, point(-97.515678, 35.512363) <@> point(lon, lat)::point AS burrito_distance
	FROM burritoplaces
	WHERE (point(-97.515678, 35.512363) <@> point(lon, lat)) < 10 --feel free to play this!
	ORDER by burrito_distance;

This returns me everything that is less than 10 miles away and orders them by distance. I see that Captain Crustacian is a half mile away and they have amazing guacamole. Thank you Postgres, cube, and earthdistance. You have saved my life once again.

If I lived anywhere else in the world, I would likely use meters and write the query as such:
	SELECT *, earth_distance(ll_to_earth(35.512363,-97.515678), ll_to_earth(lat, lon)) as burrito_distance
	FROM burritoplaces
	WHERE earth_box(ll_to_earth(35.512363,-97.515678), 4000) @> ll_to_earth(lat, lon)
	ORDER by burrito_distance;


[1]: http://www.postgresql.org/docs/current/static/earthdistance.html