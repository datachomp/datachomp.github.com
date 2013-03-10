---
title: 'Virtual Victory &#8211; AWS gives out a year of usage'
author: Rob
layout: post
permalink: /archives/virtual-victory-aws-gives-out-a-year-of-usage/
dsq_thread_id:
  - 888404928
categories:
  - AppDev
  - Databases
  - Linux
  - NoSQL
  - OS
  - SQL Server
  - Tools
---
# 

Amazon has just done something amazing... [Amazingness][1]... They are essentially doing a conditional year free of their services. While I am a huge fan of VMWare, what Amazon is offering is something completely unmatched by VCloud or MS Azure.  (Please see Craig's comment as this claim has the possibility of being debunked. Thanks Craig!)

 [1]: http://aws.amazon.com/free/

So what can I do with this you might ask? Here are some ideas: (I am doing these off the top of my head so feel free to criticize them by suggesting a better ideas)

1. If you have never played with Linux/Ruby or anything else, here is a good/free way to to dabble without the 'will this mess up my system' fear. Microsoft is continuing their adoption of features from these platforms and it can only benefit you to see these ideas they are using from Ruby(gems) or Apache(modularization). The more you know, the more you grow.

2. NOSQL? What is a NOSQL? Want a simple way to learn about NOSQL? Then try out Amazon's simpleDB. They have various demos and such you can run and play with and start trying to see what all the hype is about. Remember fellow fearful DBA's, NOSQL stands for "Not Only SQL" not for No - SQL as in SQL Void.

3. Embrace the awesome of Amazon S3. Ever wonder why Dropbox/JungleDisk and various other companies can give you so much storage for so cheap? Amazon S3 is pretty much the answer to that question. I typically use the cheapest hosting plans possible and S3 as a CDN to smoke/mirror performance.

4. SQL Server testing - Microsoft provides trial software for pretty much all their projects. If you are curious about ... say playing with some feature of replication or maybe wondering about online indexing ... Fire up 1 or more instances of EC2 and actually try it. You no longer have an excuse to put it off.

5. .Net testing or running a service - With shared hosting, I don't have a way of keeping an ongoing on demand service per say... So why not have a micro instance where you can deploy services and such. For example, say you need to add SMS services to your app.... Sign up for [Twilio][2], create a little service, throw it on your free AWS EC2 instance and poll every hour or so. Most shared hosting sites don't really support SQL Server SSIS... so why not have an AWS EC2 instance that fires up at night and starts some SSIS packages?

 [2]: http://www.twilio.com/

Now, some of this isn't without a learning curve, but we are in the software development industry... if we can't navigate learning curves or don't even pursue learning to begin with we add more nails to our career coffin as well as start becoming the problem.