---
title: Helper Classes
author: Rob
layout: post
permalink: /archives/helper-classes/
dsq_thread_id:
  - 912389094
categories:
  - AppDev
  - ASP.NET
---
# 

Trust me when I say I'm no stranger to abusing repeat methods in code behinds... however, it does reach a point where something has to give. When you reach that point, your solution is a 'helper' class.

Below is a quick/basic example of how to get started with helper classes.  
In my solution, I created a folder called 'Helpers' and added a new class object called 'StringManip'.  
Once that was completed, I added a few methods like:
<code>
	public static string UpperString(string val)
	{ return val.ToUpper(); }
</code>

In my code behind, I reference the class through a using statement:
<code>
	using HelperClass.Helpers;
</code>

And then I can call the methods:
<code>
	lbl_aspnet.Text = StringManip.UpperString(lbl_aspnet.Text);
</code>

If the above isn't enough to get you rolling.... then download the demo solution by [Clicking Here][1]

 [1]: http://files.datachomp.com/AppDev/HelperClass.zip