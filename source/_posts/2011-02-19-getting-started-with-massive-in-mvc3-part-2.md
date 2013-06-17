---
title: 'Getting Started with Massive in MVC3 &#8211; Part 2'
author: Rob
layout: post
permalink: /archives/getting-started-with-massive-in-mvc3-part-2/
dsq_thread_id:
  - 888404968
categories:
  - AppDev
  - MVC
---
# 

If you read over my first post on this you probably have some questions going through your mind.... I know I did... So lets ask them:

**Rob, your queries make no sense to me because I have no idea about this 'drinks' table you're hitting.**  
I agree. When I start to play with something I tend to use very simple databases that I know for simplicity. From here on, I'll be using [Adventure Works 2008 R2][1]

 [1]: http://msftdbprodsamples.codeplex.com/releases/view/59211

**All this stuff is dynamic and I don't know how to deal with it.** I'm just as strongly typed as the next guy so this was an emotional hurdle. But we're not here for therapy so lets code using one of my favorite examples - Credit Cards!  
Class:
<code>
	public class CreditCard : DynamicModel
		{  public CreditCard()	: base("AW_Sales_ConnStr")
			{ 
				PrimaryKeyField = "CreditCardID"; 
			}
		}
</code>
ViewModel:
<code>
	public class CreditCardViewModel
		{public IEnumerable<Cards>{ get; set; } }
</code>
Controller:
<code>
	public ActionResult Index()
	{
	      var table = new CreditCard();
	      var cards = table.All();

		var viewModel = new CreditCardViewModel
		{
	          Cards = cards
	         };
		return View(viewModel);
	}
</code>
View:
<code>
	@model Kona.Web.Models.CreditCardViewModel
	@{ViewBag.Title = "Adventure Works Card Stealer";}

	<table>
	@foreach (var item in Model.Cards)
	{
	<tbody>
	<tr>
	<td>@item.CardType</td>
	<td>@item.CardNumber</td>
	<td>@item.ExpMonth</td>
	<td>@item.ExpYear</td>
	</tr>
	}</tbody>
	</table>
</code>

So 2 seconds after this works, I hope that you are feeling very uncomfortable about presenting Credit Card numbers or even pulling them. That is a good thing.... So since we have a special case where we care a whole bunch about not returning that data... lets change our controller:
<code>
	var table = new CreditCard();
	//var cards = table.All();
	var cards = table.Query(@"SELECT CardType, ExpMonth ,ExpYear from CreditCard");
</code>
Sweet! it compiles, we're ready to check in and go grab a beer right? Heh, nope.... refresh your app:  
*'System.Dynamic.ExpandoObject' does not contain a definition for 'CardNumber'*

So we update our view....
<code>
	{
	@item.CardType
	@item.ExpMonth
	@item.ExpYear
	}
</code>

and all is well again. 

Make no mistake about it... Massive will give you all the rope you want to easily swing around your apps data jungle... it will also give you all the rope you want to hang yourself if that is what you choose to do with it. Which is precisely why I like it. I feel like Entity Framework4 ([And don't get me wrong, I like EF4][3]) is a micromanaging boss that wants to check my every move. Massive is like the boss that gives me a task and trusts that I'll get it done.

 [3]: http://thisdeveloperslife.com/post/1-0-8-motivation

Welcome to dynamic!

One last bit because this is getting lengthy:  
**Credit cards in AdventureWorks 2008 R2 are locked into a Sales schema and I can't figure out how make my class take "advantage" of that**  
This is a fun thing we DBA's like to do for the sole purpose of driving AppDev's nuts (with just some oh so minor security and organizational reasoning behind it). In our Credit Card model we can update to specify the TableName. Also, I use a special connection string:
<code>
	public CreditCard() : base("AW_Sales_ConnStr")
			{ 
				TableName = "[Sales].[CreditCard]";
				PrimaryKeyField = "CreditCardID"; 
			}
</code>

That setting in the base() maps to my web.config:
<code>
	add name="AW_Sales_ConnStr" connectionString="Data Source=localhost;Initial Catalog=AdventureWorks2008R2;User ID=aw_Kona_Sales;Password=Sales_Password;Application Name=KonaSales;" providerName="System.Data.SqlClient"
</code>

and in SQL Server this maps to the Sales Schema (key part being the specification of a Default_Schema:
<code>
	CREATE USER [aw_Kona_Sales] FOR LOGIN [aw_Kona_Sales] WITH DEFAULT_SCHEMA=[Sales]
</code>

Last thing, I promise ... In my first post, I included how the commands were rendering out in the SQL Server. I didn't this time because this edition is more focused on a "getting things done in MVC" side of things.  
Trust me, I am a DBA first ( and I imagine some of my code probably shows it) and I am still profiling every single thing I do with Massive looking for problems.