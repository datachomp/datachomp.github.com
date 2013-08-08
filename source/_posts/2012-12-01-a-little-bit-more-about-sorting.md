---
title: A little bit more about sorting
author: Rob
layout: post
permalink: /archives/a-little-bit-more-about-sorting/
dsq_thread_id:
  - 953027716
categories:
  - AppDev
---
# 

As a big fan of not [sorting in the database][1] when I don't have to, I have often resorted to heavy handed javascript libraries like [datatables.net][2]. I do this in large part because I am just awful at javascript and the libs typically already exists. As long as the database is saving a few cycles who cares about the client right?!?! 

 [1]: http://datachomp.com/archives/hey-app-quit-wasting-my-time-sorting-your-data/ "Don't waste my time"
 [2]: http://datatables.net/ "datatables.net"

This time of year is all about giving, so I thought I would try to find a better/lighter way of doing this. A few googles later, I found the [tablesorter][3] lib and this looked like a pretty decent compromise.

 [3]: http://tablesorter.com/docs/ "tablesorter"

Thinking I was onto something, I ran the idea by my resident javascript expert Michael Sarchet ([b][4] / [t][5]). His response was so simple and on point that I was slightly offended I hadn't thought of it. "I just use [Knockout][6] for that." Advantages of this are that I'm not having to keep up with another lib (knockout is a default lib in MVC4) and this solution will be able to apply to collections outside of just a table. Thus, another life skill was born.

 [4]: http://www.michaelsarchet.com/ "ha mikes blog"
 [5]: https://twitter.com/msarchet "Mike"
 [6]: http://knockoutjs.com/ "KnockoutJS"

**Code Time  
Controller:**
<code>
    var burritos = db.Select("select name,price from burritos");
    var tacos = db.Select("select name,price from tacos");
                
    var viewModel = new viewmodel_Food
    {
      burritos = burritos
      , tacos = tacos
    };
    return View(viewModel);
</code>
**View:**
<code>
    @model burritoroll.web.Models.viewmodel_Food
    @{
        ViewBag.Title = "Index";
    }

    Index
 
    Can you dig our burritos?
    BurritoPrice
     
      
     
    Can you dig our tacos?
    
     
      TacoPrice
     
     
      
     
    
    @section scripts{
    	
    	function viewModel() {
    		var self = this;
    
    	    self.tacosarray = ko.observableArray(@Html.Raw(Json.Encode(Model.tacos.ToArray())));
    	    self.burritosarray = ko.observableArray(@Html.Raw(Json.Encode(Model.burritos.ToArray())));
    	    
    	    var sortitFunction = function (a, b) {
    	        return a.name.toLowerCase() == b.name.toLowerCase() ? 0 : a.name.toLowerCase() < b.name.toLowerCase() ? -1 : 1;
    	    };
    
    	    self.tacosarray.sort(sortitFunction);
    	    self.burritosarray.sort(sortitFunction);
    	}
    
    	ko.applyBindings(new viewModel());
    	
    }
</code>