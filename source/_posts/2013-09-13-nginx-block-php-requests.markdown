---
layout: post
title: "nginx block php requests"
date: 2013-09-13 22:28
comments: true
categories: 
---
### problem
When your rails app barfs, it isn't pretty. Even with a basic routing error, you can count on another 30 lines of junk you don't care about. A lot of these junk requests come from bots making php requests or just looking for low hanging fruit exploits. We need something to tune the signal to noise in our application logs as well as not wasting our apps time on bots. 

### solution
Just throw in a quick nginx rule to catch and handle all the requests before they even hit your server.


	location ~ (\.php|.aspx|.asp|myadmin) {
		return 404;
	}