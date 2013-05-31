---
title: You Literaled my CSS
author: Rob
layout: post
permalink: /archives/you-literaled-my-css/
dsq_thread_id:
  - 893326380
categories:
  - ASP.NET
---
# 

On the main F5 page, I needed a way to adjust the CSS properties on the menu based on what is in a content page.  Enter ASP.NET literals.  Essentially a literal lives up to its name... it allows you to define literal html.

So, in the master I have:

<code>
	<asp:Literal ID="lit_About" runat=server Text='<li><a href="About.aspx" title="About">About</a></li>' />
<code>

 

How do I manipulate that to trigger the proper CSS class you ask?  By doing this in the code behind on the content page:
<code>
	protected void Page_Load(object sender, EventArgs e) 
	{ 
	Literal menucontrol = Page.Master.FindControl("lit_About") as Literal; 
	menucontrol.Text = "<li class='active'><a href='About.aspx' title='About'>About</a></li>";  
	}
</code>