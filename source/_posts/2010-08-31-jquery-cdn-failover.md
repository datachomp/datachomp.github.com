---
title: JQuery CDN failover
author: Rob
layout: post
permalink: /archives/jquery-cdn-failover/
dsq_thread_id:
  - 890715297
categories:
  - AppDev
  - ASP.NET
---
# 

With 'high availability' being a nicely stressed buzzword coupled with issues in the 'cloud', an issue can arise of what to do about your jquery references in the event that where you are hosting it is down? I won't dig into the reasons for using a CDN for jquery but will plug the usual - It will typically be cached by the time your site is hit if you use Google or Microsoft CDN as well as will likely be faster to load in general as a browser will thread out to load it (as opposed to if it is hosted under the current domain). I also have some links at the bottom for further reading on it. So, on to the failover.... and it is quite easy really since 1.4 :
<code>
	<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js"></script>
	<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.4/jquery-ui.js"></script>

	<script type="text/javascript">
	  if (typeof jQuery == 'undefined')
	  {
	     document.write(unescape("%3Cscript src='http://ajax.microsoft.com/ajax/jquery/jquery-1.4.4.min.js' type='text/javascript'%3E%3C/script%3E"));
	     document.write(unescape("%3Cscript src='http://ajax.microsoft.com/ajax/jquery/jqueryui-1.8.4.min.js' type='text/javascript'%3E%3C/script%3E"));
	  }
	  </script>
</code>


