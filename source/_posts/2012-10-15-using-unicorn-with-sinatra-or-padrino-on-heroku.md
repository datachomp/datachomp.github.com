---
title: Using Unicorn with Sinatra or Padrino on Heroku
author: Rob
layout: post
permalink: /archives/using-unicorn-with-sinatra-or-padrino-on-heroku/
dsq_thread_id:
  - 888330794
categories:
  - AppDev
---
# 

My apps on [Heroku][1] have been cruising along ok using Thin as my rack server, but on my [VPS boxes][2], [Unicorn][3]. I was hoping to keep a more consistent experience for myself and as it turns out, getting my Heroku apps to run on Unicorn is pretty simple and runs really fast.

 [1]: http://www.heroku.com/ "Heroku"
 [2]: http://www.lowendbox.com/
 [3]: http://unicorn.bogomips.org/ "Unicorn"

1st: I added a Unicorn config file: **touch config/unicorn.rb** and added the following:  
<code>worker_processes 3  
timeout 30  
preload_app true</code>

2nd: I added Unicorn to my GemFile:  
<code>#rack server  
gem 'unicorn'</code>
You will want to  "bundle install" after that step so  that it gets picked up and added to your Gemfile.lock  
  
3rd: I added a Procfile to the root of my app: **touch Procfile** and added the following:  
<code>web: bundle exec unicorn -p $PORT -c ./config/unicorn.rb</code>

Commit it into Git, then git push heroku branchname and you are enjoying the sweet sweet threading love of Unicorn.