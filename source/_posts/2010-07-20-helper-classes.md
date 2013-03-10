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

    1.  &nbsp;
    
    2.  public static string UpperString&#40;string val&#41;
    
    3.  &#123; return val.ToUpper&#40;&#41;; &#125; 
    
    4.  &nbsp;

In my code behind, I reference the class through a using statement:

    1.  &nbsp;
    
    2.  using HelperClass.Helpers;
    
    3.  &nbsp;

And then I can call the methods:

    1.  &nbsp;
    
    2.  lbl_aspnet.Text = StringManip.UpperString&#40;lbl_aspnet.Text&#41;;
    
    3.  &nbsp;

If the above isn't enough to get you rolling.... then download the demo solution by [Clicking Here][1]

 [1]: http://files.datachomp.com/AppDev/HelperClass.zip