---
layout: post
title: "Passenger: No Longer in /opt/ion"
date: 2013-08-27 23:26
comments: true
categories: 
---

UPDATE on official repos - http://blog.phusion.nl/2013/09/11/debian-and-ubuntu-packages-for-phusion-passenger/  
  
When you are getting started in the world of Ruby web dev, one of the best things you can do is deploy to [Heroku][1].  However, if you find yourself having to deploy to a [VPS][3], one of next best things you can do is use [Phusion Passenger][2]. It is an incredibly low friction application server for rack apps (and other things).

[1]: http://heroku.com
[2]: https://www.phusionpassenger.com/
[3]: http://linode.com

### Magic Strings of Wonderful Glory

After you get your feet under you, you might find yourself needing to use more than the base modules that come with the nginx inside Passenger. As I would go to install or update Passenger, I would end up having to run something like this:  
  

	gem install passenger
  
	passenger-install-nginx-module --nginx-source-dir=/usr/src/nginx-1.5.3 ---with-http_ssl_module --with-http_gzip_static_module --with-http_stub_status_module --with --without-http_scgi_module --without-http_uwsgi_module --without-http_fastcgi_module --without-mail_pop3_module --without-mail_imap_module --without-mail_smtp_module --with-http_realip_module --add-module=/opt/nginxmodules/headers-more
  
  
This creates a few issues. Issue #1: I have to keep wget'ing new versions of source when I need to update.  
Issue #2: I have a completely different workflow/scripts for managing a passenger nginx instance than I do for a unicorn/puma instance. This variance drives me nuts and be a source of script churn.

### Antacids

I kept thinking there had to be a better way. Then the sky hash parted and a beam of light shown directly on a ppa from the brightbox team. This package uses a traditional installation of nginx, which means that I can easily install the nginx-extras package and get all my extra modules I need for load balancing and nginx_status. Here is how I do it:

	sudo apt-get install -y python-software-properties curl git-core  
	sudo apt-add-repository -y ppa:brightbox/passenger-experimental  
	sudo apt-get update
  
	sudo apt-get install -y nginx-full  
	sudo apt-get install -y nginx-extras  #extras headers and stuff  
  
	\curl -L https://get.rvm.io | bash  
	source ~/.profile  
  
	rvm install 2.0.0-p247  
	rvm --default use 2.0.0-p247  
  
	sudo service nginx restart  
  
Most of how to do this is covered at:  
[http://www.modrails.com/documentation/Users%20guide%20Nginx.html#rubygems_generic_install][4]

[4]: http://www.modrails.com/documentation/Users%20guide%20Nginx.html#rubygems_generic_install


