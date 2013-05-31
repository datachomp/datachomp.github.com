---
title: 'Early MVC Pitfalls &#8211; Shared Hosting and Action Link'
author: Rob
layout: post
permalink: /archives/early-mvc-pitfalls-shared-hosting-and-action-link/
dsq_thread_id:
  - 888404921
categories:
  - AppDev
  - MVC
---
# 

I have a great hosting provider - [Arvixe][1] and they are on the ball for both my Linux hosting as well as my .NET stuff. So as I started cranking out apps and went to deploy I knew I could count on them to handle the hosting task at hand. So, compile... deploy... and we're erroring out. Oh! I forgot to set my site to integrated pipeline.... fix that, drop some control f5 and still an issue. So, CustomErrors=OFF, deploy and see that it is an error with the System.Web.MVC assembly. That's odd, well a google searching showed that this is a bit of known error. To fix, you simply click on the properties of that assembly, and set the "copy local" property to true. More information can be found here:  
[This dudes blog][2]  
and from Haacked himself: [clickey][3]

 [1]: http://www.arvixe.com/
 [2]: http://www.britishdeveloper.co.uk/2010/05/mvc2-deploy-could-not-load-file-or.html
 [3]: http://haacked.com/archive/2008/11/03/bin-deploy-aspnetmvc.aspx

MVC ActionLink returns ?Length= - This was a real WTF for me. Turns out that if you specify a htmlattributes overload for ActionLink and don't put in something like null for the route values overload, it will append some stupid length=# string to your href.

<code>
	<%: Html.ActionLink("Home", "Index", "Home", new { @class = "active" })%>
</code>

Generates the following html: 
<code>
	<a class="active" href="/?Length=4">Home</a>
</code>

So we throw in null for the routeValues overload:
<code>
	<%: Html.ActionLink("Home", "Index", "Home", null ,new { @class = "active" })%>
</code> 

and we get the html we want.