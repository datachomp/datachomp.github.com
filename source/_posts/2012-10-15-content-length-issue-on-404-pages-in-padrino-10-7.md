---
title: Content length issue on 404 pages in Padrino 10.7
author: Rob
layout: post
permalink: /archives/content-length-issue-on-404-pages-in-padrino-10-7/
dsq_thread_id:
  - 888642001
categories:
  - AppDev
---
# 

I've been tooling around with [PadrinoRB][1] lately. Personally, it has been a nice mix of [SinatraRB][2] and Rails. One of the annoying issues I've hit is that when I define a view for my 404 errors:  
`
error 404 do
  response.status = 404
  render 'errors/404', :layout=>:applayout
end
`  
It will only show 30 characters of the defined view. Luckily, in this day in age, Google has all the answers and I found out [here][3] that in my Gemfile I need to define a certain version of Sinatra for this to work. So I popped open my Gemfile and added the following:  
`
gem 'rake'
gem 'sinatra', '1.3.2'
gem 'sinatra-flash', :require => 'sinatra/flash'
`  
Once I locked in 1.3.2 instead of the 1.3.3 version of sinatra I was using, it worked like a charm and I stopped looking like a complete idiot that doesn't now how to serve up error pages properly.

 [1]: http://www.padrinorb.com/ "PadrinoRB"
 [2]: http://www.sinatrarb.com/ "Sinatrarb"
 [3]: https://groups.google.com/forum/?fromgroups=#!topic/padrino/ThPy9U4sK4w "here"