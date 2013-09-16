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
  
For those curious what I'm talking about -  
Here is an application log before:
	Started GET "/login.php" for 80.241.16.10 at 2013-09-13 09:01:38 +0000

	ActionController::RoutingError (No route matches [GET] "/login.php"):
  	actionpack (3.2.14) lib/action_dispatch/middleware/debug_exceptions.rb:21:in `call'
  	actionpack (3.2.14) lib/action_dispatch/middleware/show_exceptions.rb:56:in `call'
  	railties (3.2.14) lib/rails/rack/logger.rb:32:in `call_app'
  	railties (3.2.14) lib/rails/rack/logger.rb:16:in `block in call'
  	activesupport (3.2.14) lib/active_support/tagged_logging.rb:22:in `tagged'
  	railties (3.2.14) lib/rails/rack/logger.rb:16:in `call'
  	actionpack (3.2.14) lib/action_dispatch/middleware/request_id.rb:22:in `call'
  	rack (1.4.5) lib/rack/methodoverride.rb:21:in `call'
  	rack (1.4.5) lib/rack/runtime.rb:17:in `call'
  	rack (1.4.5) lib/rack/lock.rb:15:in `call'
  	rack-cache (1.2) lib/rack/cache/context.rb:136:in `forward'
  	rack-cache (1.2) lib/rack/cache/context.rb:245:in `fetch'
  	rack-cache (1.2) lib/rack/cache/context.rb:185:in `lookup'
  	rack-cache (1.2) lib/rack/cache/context.rb:66:in `call!'
  	rack-cache (1.2) lib/rack/cache/context.rb:51:in `call'
  	rack-mini-profiler (0.1.29) Ruby/lib/mini_profiler/profiler.rb:188:in `call'
  	railties (3.2.14) lib/rails/engine.rb:484:in `call'
  	railties (3.2.14) lib/rails/application.rb:231:in `call'
  	railties (3.2.14) lib/rails/railtie/configurable.rb:30:in `method_missing'
  	and on... and on... and on ...

Here is one after:
<pre><code>	 

</code></pre>

