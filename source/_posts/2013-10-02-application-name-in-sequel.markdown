---
layout: post
title: "application_name in sequel"
date: 2013-10-02 09:22
comments: true
categories: 
---

When I was in the .net world, I was pretty adamant about the importance of giving your App a nice [application_name][1] to help out your DBA. Now that I'm in the ruby/postgres world, the same still holds true.  
  
As much as I want to like ActiveRecord, I can't seem to get past anything more than a casual friendship with it. My tool of choice is [Sequel][3]. One of the problems with Sequel is that setting the application name isn't very intuitive. Luckily, the awesome maintainer [Jeremy Evans][2], pointed me in the right direction with the :after_connect option. Check it out:

	require 'sequel'
	DB = Sequel.connect(:adapter=>'postgres', :host=> 'localhost'
	, :database=>'datachomp', :user=>'rob', :max_connections => 10
	, :after_connect=>(proc do |conn| conn.execute("SET application_name TO 'dbahugs'") end))

 When I look at my pg_stat_activity, I'm able to filter down properly and predictably:
 	select * from pg_stat_activity where application_name = 'dbahugs'


[1]: http://datachomp.com/archives/application-connection-ocd/
[2]: https://twitter.com/jeremyevans0
[3]: https://github.com/jeremyevans/sequel