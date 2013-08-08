---
title: Running ASP.NET MVC4 on Ubuntu 12.04
author: Rob
layout: post
permalink: /archives/running-asp-net-mvc4-on-ubuntu-12-04/
dsq_thread_id:
  - 888334258
categories:
  - AppDev
  - Linux
  - MVC
  - OS
---
# 

The single most question I get regarding our [Hello Postgres][1] series over Tekpub is "How are you running your app on Linux?" As much as I enjoy sending volleys of fragmented explanations over emails to people, I decided it might be in everyone's best interest if I just blog it out. Technology moves fast... like super fast. So instead of showing you how to deploy an old and busted MVC3 app to a "has been" Ubuntu 11.10, we're going to be going with MVC4 and [Ubuntu 12.04][2]. We're so bleeding edge you can call us cutters!

 [1]: http://tekpub.com/productions/pg "Hello Postgres"
 [2]: https://wiki.ubuntu.com/PrecisePangolin/TechnicalOverview/Beta2 "Ubuntu 12.04 beta 2"

I'm not going to show you how to set up and install your Ubuntu Server, as there is a ton of internet out there that can handle that for you and the installer itself is pretty hard to mess up. What I will cover is what to do after that last reboot where it is just you and Bash, together... at last.

SSH into your fancy new Ubuntu box with [Putty][3] and Run this:  
<code>sudo apt-get update && sudo apt-get -y install git-core curl python-software-properties</code>

 [3]: http://www.chiark.greenend.org.uk/~sgtatham/putty/ "Putty"

This is going to let us add the ppa(think of this as a package location) to install a current nginx:  
<code>
	sudo add-apt-repository ppa:nginx/stable
	sudo apt-get update && sudo apt-get -y install nginx
</code>

Yes! We can now git things, we can curl things, and we can nginx things! Rawwrr!!! Let's kick some other processes to the curb... like that skank MySQL and that wet blanket Apache:  
<code>sudo update-rc.d mysql remove &&  
sudo update-rc.d apache2 remove && mkdir mycoolapp</code>

Now for the fun part, let's get Mono up and running. We really want the latest version of Mono, and like Nuget, not all packages in Ubuntu stay current. The fix for this in the mono world is to just grab [this dudes][4] install script and go have a drink because it takes a bit to finish:  
<code>
	mkdir mono-2.11
	cd mono-2.11
	wget --no-check-certificate https://github.com/nathanb/iws-snippets/raw/master/mono-install-scripts/ubuntu/install_mono-2.11.sh
	chmod 755 install_mono-2.11.sh
	./install_mono-2.11.sh
</code>

 [4]: http://www.integratedwebsystems.com/2012/04/install-mono-2-11/ "Nathan Bridgewater"

Now to check that we have the current version, we just run the following:  
<code>/opt/mono-2.11/bin/mono -V</code>

Cool! We're current, but what we really need is to just be able to run "mono -V" anywhere we want. We can do that by modifying our /etc/environment file:  
<code>sudo nano /etc/environment</code>
#then insert ":/opt/mono-2.11/bin"  at the end of that path string. Log out, log in for it to take effect.`

I don't now about you, but all this sys-admin crap is putting me to sleep and I want to code. Back on our Widows/Dev box, we're going to new up an empty MVC4 project (up to you to figure out how to get MVC4 going on your box, I used Web Platform Installer). After the project is new'ed up:

*   Add a controller
*   Add a view based on that controller
*   Hit F5 and make sure it works
*   Remove the EntityFramework.dll because we love our data and don't want to give our app a low self esteem
*   Once removed, hit F5 again

If everything is still running, then do a file system deploy to a local folder of your choice. Go check out what was just deployed, specifically the '/bin' folder. We want to remove the Web.Infrastructure.dll because Mono already has this built into its GAC. High Five Mono! With that out of the way, we are going to copy that deployment over to the 'mycoolapp' folder on our Linux server using [WinsCP][5]. While I don't have a WordPress plugin to read your mind, I hear you thinking "Rob, surely there are better ways to do this." and you are right. However, this post is long enough and that topic will likely go into its own post.

 [5]: http://winscp.net/eng/index.php "Winscp"

SSH back into your linux box, and 'cd mycoolapp'. "cd" into your "mycoolapp" directory, type in 'ls' to see the contents and make sure your app made it over. Once we see it, we can do a cassini like run of it by typeing out xsp4 and hitting enter. Try and hit your box on the port 8080 (xsp4 should tell you what it is running on) and see if you can see your app:

![][6]

 [6]: http://files.datachomp.com/AppDev/mono/monoapp.png

Yay! Our code works and we're so open source now that we can wear tight jeans at Starbucks and put Bon Iver on our ipod... just like those [Rails wankers][7]! As for nginx and running our site as a Mono service, we can just use some of the nice documentation over at the Mono site:

 [7]: http://wekeroad.com/ "Lead Rails Wanker"

Nginx config:  
[http://www.mono-project.com/FastCGI_Nginx][8]

 [8]: http://www.mono-project.com/FastCGI_Nginx "FastCGI_Nginx"

Mono as a service:  
[http://yojimbo87.github.com/2010/03/14/mono-startup-script.html][9]

 [9]: http://yojimbo87.github.com/2010/03/14/mono-startup-script.html "Monoserve"