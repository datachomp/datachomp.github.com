---
layout: post
title: "nginx block php requests"
date: 2013-09-13 22:28
comments: true
categories: 
---
### problem:
When your rails app barfs on an error, what goes into the app logs isn't pretty. Even with a basic routing error, you can count on 30 lines or so of stack junk going into your application log - much of what is inserted you don't care about. In a normal, healthy, fun loving rails app, the majority of the routing errors come from script kiddies or an exec's infected home machine making php requests. These requests are just casting out a broad net and looking for low hanging fruit exploits. We need something to help tune the signal to noise occuring in our application logs as well as to stop wasting our apps time on bots. 

### solution:
Just throw in a quick nginx location rule in your apps nginx config to catch and handle all the requests before they even hit your server.


	location ~ (\.php|.aspx|.asp|myadmin) {
		return 404;
	}