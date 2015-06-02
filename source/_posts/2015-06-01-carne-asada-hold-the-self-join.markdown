---
layout: post
title: "Carne Asada hold the self join"
date: 2015-06-01 21:57:34 -0500
comments: true
categories:
---
Since leaving the tech industry, I've been disrupting the food service sector with my reimaginification of the food truck. Basically, I serve freshly heated burritos out of the back of my car. The company is called "Uber-ritos" and things could not be any better... well actually, there is this one thing.

#### Disrupting the Food Line
The point of sale system in the back of my car runs off a repurposed TI-82. While it has everything one might need to run a legit company (keyboard/ display/postgres) there is still quite the penalty for wasting CPU cycles. Like most Enterprise system,s it has a very noticeable hot spot. Let us see if we can fix it!

#### Plumbing
```
CREATE TABLE food_line(order_id int, step_number int, step_name text, completed_at timestamp);
INSERT INTO  food_line(order_id, step_number, step_name, completed_at)
VALUES (1, 1, 'heat tortilla', now()), (1, 2, 'load with food', null), (1,3, 'serve customer', null)
, (2, 1, 'heat tortilla', now()), (2, 2, 'load with food', now()), (2,3, 'serve customer', null)
, (3, 1, 'heat tortilla', now()), (3, 2, 'load with food', null), (3, 3, 'serve customer', null)
, (4, 1, 'heat tortilla', now()), (4, 2, 'load with food', null), (4, 3, 'serve customer', null)
, (5, 1, 'pour queso', now()), (5, 2, 'serve customer', null);
```
#### SubQuery V1
```
EXPLAIN SELECT fl.order_id, fl.step_name
FROM food_line fl
	INNER JOIN
	(SELECT order_id, min(step_number) as min_step_number
	FROM food_line
	WHERE completed_at is null
	group by order_id
	) hu on fl.order_id = hu.order_id
			AND fl.step_number=hu.min_step_number
ORDER BY fl.order_id;
```
**"Sort  (cost=48.68..48.69 rows=1 width=36)" **  
Above is the first version of the "What's left" query and while it worked well enough, I didn't think it was that readable. My business deserves a CTE version.

#### CTE Version
```
EXPLAIN WITH last_step as (
	SELECT order_id, min(step_number) as min_step_number
	FROM food_line
	WHERE completed_at is null
	GROUP BY order_id)
SELECT fl.order_id, fl.step_name
FROM food_line fl
	INNER JOIN last_step ls on fl.order_id = ls.order_id
		   AND fl.step_number=ls.min_step_number
ORDER BY order_id;
```
** "Sort  (cost=48.69..48.70 rows=1 width=36)" **  
I find this version much more readable. I can alias the CTE and tweak the last step process. It also doesn't affect cost a whole heck of a lot...but cost is what I really need to lower.

#### Order Up!!! Window Function
```
EXPLAIN SELECT order_id, step_name
FROM (
	SELECT order_id, step_name, row_number()
      OVER (PARTITION BY order_id ORDER BY step_number) as stuck_step
	FROM food_line
	WHERE  completed_at is null) as data
WHERE stuck_step = 1
ORDER BY order_id
```
**"Subquery Scan on data  (cost=20.46..20.62 rows=1 width=36)"**  
Wow! Half the cost! This new found efficiency lets me serve an extra 15 burritos per lunch cycle which means more revenue! This additional income will let me decide if I want to pocket the extra money, or reinvest it back into the company. I might reinvest in something like upgrading the point of sale to a raspberry pi and fancy display.  With more horsepower I can tackle some of the items the business unit wants but customers hate - like facebook/twitter integration!
