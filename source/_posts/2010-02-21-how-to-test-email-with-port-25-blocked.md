---
title: How to test email with port 25 blocked
author: Rob
layout: post
permalink: /archives/how-to-test-email-with-port-25-blocked/
dsq_thread_id:
  - 979450134
categories:
  - AppDev
---
# 

Recently, I got rid of the business line to my house. The downside to this is losing ports 25 and 80. Port 80 isn't a real deal breaker as when I'm coding/debugging I'm just using a local webserver. However, when I am testing email functionality in an application things get a little more interesting. The fix I found for this involves how you specify the SMTP actions in the webconfig.

In your Configuration section, you can specify either network delivery or a pickup location:  
<code>
	<system.net>
		<mailSettings>	 <!--<smtp deliveryMethod="SpecifiedPickupDirectory"><specifiedPickupDirectory pickupDirectoryLocation="C:SubversionEmail" /></smtp>-->	
			<smtp deliveryMethod="Network"><network host="datachomp.com" port="25" defaultCredentials="true" userName="Gator@example.com" password="email" /></smtp> 
		</mailSettings>
	</system.net>
</code>
At this point you just check the local directory for email when you are testing your application. If you don't have system for production deployments, then don't forget to change to the network delivery when you deploy.