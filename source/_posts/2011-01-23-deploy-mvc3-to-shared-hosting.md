---
title: Deploy MVC3 to Shared Hosting
author: Rob
layout: post
permalink: /archives/deploy-mvc3-to-shared-hosting/
dsq_thread_id:
  - 889028689
categories:
  - AppDev
  - ASP.NET
---
# 

Deploying ASP MVC3 to shared hosting isn't always sunshine and lollipops.   Though my beloved hosting company [Arvixe][1] is pretty cutting edge,  it didn't have (and I didn't request) that they hop on MVC3 or its beta.   Regardless, I went to deploy an MVC3 app and it blew up because of a variety of dependencies.   As [discovered][2] in early MVC2 work, you have to set some assemblies to 'copy local' to get it to work on shared hosting.

 [1]: http://www.arvixe.com/851.html
 [2]: http://datachomp.com/archives/early-mvc-pitfalls-shared-hosting-and-action-link/ "discovered"

The list of assemblies to set to copy local = true is:

*   Microsoft.Web.Infrastructure
*   System.Web.Helpers
*   System.Web.Mvc
*   System.Web.Razor
*   System.Web.Webpages
*   System.Web.WebPages.Deployment
*   System.Web.Webpages.Razor

Drew Miller ( [blog][3] | [twitter][4] ) has a great breakdown on this : [Click for Breakdown ][5]

 [3]: http://drew-prog.blogspot.com/
 [4]: http://twitter.com/anglicangeek
 [5]: http://drew-prog.blogspot.com/2011/01/how-to-deploy-aspnet-mvc-3-app-to-web.html