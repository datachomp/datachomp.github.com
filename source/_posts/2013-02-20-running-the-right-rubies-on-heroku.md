---
title: Running the right rubies on Heroku
author: Rob
layout: post
permalink: /archives/running-the-right-rubies-on-heroku/
dsq_thread_id:
  - 1096131368
categories:
  - AppDev
---
# 

Being new to Ruby I have the luxury of running the latest rubies without much brownfield drama. With all the security concerns of late, I have never been more self righteous in my use of the latest and greatest in both rack and ruby. On my various servers, this has been very easy to do with RVM and my Gemfile. On Heroku, I realized that I had been running 1.9.2 instead of 1.9.3. The fix is easy enough though, as you just add which ruby you want to run to your Gemfile like so:  
`
source "https://rubygems.org"
ruby '1.9.3'
`

You can get a list of the latest rubies supported by Heroku here:  
[Heroku Rubies][1]

 [1]: https://devcenter.heroku.com/articles/ruby-support#ruby-versions "Heroku Rubies"

If you are an RVM user like myself, feel free to add an .rvmrc file that will match what version Heroku is on for even more consistency goodness.  
`
rvm --create --rvmrc 1.9.3-p327@mycoolproject
`