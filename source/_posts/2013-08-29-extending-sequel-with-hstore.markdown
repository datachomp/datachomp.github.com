---
layout: post
title: "Extending Sequel with hstore"
date: 2013-08-29 21:19
comments: true
categories: 
---
I've always been a fan of the cleaner simpler ORMs. When I was getting started with Ruby, [Active Record][1] was like putting my App in a fat suit. In the search for something leaner, I found [Sequel][2]. It reminded me a lot of [Massive][6] in .NET and at that point, everything in this world of Ruby started clicking.

### Iocane Powder
Now that I am 100% on Postgres, Sequel can be made even sweeter (ie: faster) with the [sequel_pg][3] gem. Like Iocane Powder, sequel_pg has no smell which means that you don't have to change any of your existing code. All you need to do is declare it in your Gemfile and get back to coding. If you are using Postgres, then your App will have a built up immunity to Iocane Powder making it safe to use. If your life blood is coursing with MySQL then adding sequel_pg can be an extremely fatal move. Though, that may not be a bad thing. *zing!*

### Say Hello To My Little Friend
Part of the absolute joy of using an object relational database like Postgres is... well, using objects. One of the coolest ways to turn our DB into Robocop is with an extension called [hstore][4] which allows for a Key-Value store inside our DB. Here is how flip the hstore switch and insert some data:  


	CREATE EXTENSION IF NOT EXISTS hstore;  
	CREATE TABLE kvburritos (id serial not null, details hstore not null, CONSTRAINT kvburritos_pkey PRIMARY KEY (id));
  
	INSERT INTO kvburritos (details)
	VALUES ('burritoname => "Postgres", toppings => "chicken, cheese, avacado, peppers", price=> 4.50')
	, ('burritoname => "MS Server", toppings => "chicken", price => "15.50"')
	, ('burritoname => "Redis", toppings => "steak, cheese, spinach", price => "3.50"')
	, ('burritoname => "Rethink", toppings => "egg, chorizo, bacon", price => "5.00"')
	, ('burritoname => "Oracle", toppings => "gold, diamonds, elitism", price => "Call for a Quote"'); -- don't run this
  
Yay! I'm not going to do any more examples in SQL since there are plenty to go around (or just use the documentation linked above). The problem we are faced with now, is that SQL does not necessarily help us out with Sequel...have fun reading that sentence out loud. Fear not, [Jeremy Evans][5] and friends already have our back with extensions you can apply to Sequel. Check it out:


	require 'sequel'
	Sequel.extension(:pg_hstore, :pg_hstore_ops, :pg_array)
	DB = Sequel.connect(:adapter=>'postgres', :host=> 'localhost', :database=>'burritomix', :user=>'kencollins')

I put that array one on there for free. When we use this in our app, instead of a gutless, soulless, faceless hstore hash from the DB, we're getting awesome, tangible, huggable data:

	#our model:
	#get the burritos that have cheese on them. Replace with Avacado in Production
	@cheesylist = DB.fetch("SELECT * FROM kvburritos WHERE lower(details->'toppings') LIKE '%cheese%';")

	#our view:
	<h2>So Cheezy</h2>
      <ul>
      <% @cheesylist.each do |c| %>
	  <li><%= c[:details][:burritoname] %> - <%=  c[:details][:toppings] %></li>
      <% end %>
      </ul>
  
### Be still my beating DAL
I love this! We can now build as fast as we want with semi structured data... and when our data becomes important and matures a little bit, we can easily break into relational schemas - Just as God intended.


[1]: http://www.comiclist.com/media/blogs/news/AngryStayPuftBank.jpg
[2]: https://github.com/jeremyevans/sequel
[3]: https://github.com/jeremyevans/sequel_pg
[4]: http://www.postgresql.org/docs/current/static/hstore.html
[5]: https://twitter.com/jeremyevans0
[6]: https://github.com/robconery/massive
