---
layout: post
title: "Query Archive Database"
date: 2015-06-08 23:23:54 -0500
comments: true
categories:
---

When talking to people about data archival strategies, a question that comes up regularly is ‘how do I even get the archived data?’  This is in part because ~modern~ web frameworks like Ruby-on-Rails don’t always make multiple data sources a straight forward task. True to form, for every problem a web framework can create, postgres can usuallyly fix it.

Say we have a main database ‘burrito_store’ and an archive database ‘burrito_archive’. Out solution will work whether ’burrito_archive’ could be on the same server, or it might be on a neighboring server, but for scenario, it will be on the same server.

#### Plumbing:
```
CREATE DATABASE burrito_store;
CREATE DATABASE burrito_archive;

\c burrito_store
CREATE TABLE burrito_sales (id serial primary key, customer_id int, sale_amount int, created_at timestamp, updated_at timestamp)

INSERT INTO burrito_sales(customer_id, sale_amount, created_at, updated_at)
values(1, 123, now(), now()), (1, 123, now(), now()),(1, 123, now(), now())
,(2, 123, now(), now()),(2, 123, now(), now()) ,(2, 123, now(), now())
,(3, 123, now(), now()),(4, 123, now(), now())

CREATE EXTENSION postgres_fdw;
CREATE SERVER archive
 FOREIGN DATA WRAPPER postgres_fdw
  OPTIONS (host 'localhost', port '5432', dbname 'burrito_archive');

CREATE USER MAPPING FOR public SERVER archive OPTIONS (user 'rob');
CREATE FOREIGN TABLE sales_archive (id int, customer_id integer, sale_amount int, created_at timestamp, updated_at timestamp)
 SERVER archive OPTIONS (schema_name 'public', table_name 'burrito_sales_archive');
```
#### On the archive database:  
```
CREATE TABLE burrito_sales_archive (id int primary key, customer_id int, sale_amount int, created_at timestamp, updated_at timestamp)

CREATE EXTENSION postgres_fdw;
CREATE SERVER store
 FOREIGN DATA WRAPPER postgres_fdw
  OPTIONS (host 'localhost', port '5432', dbname 'burrito_store');

CREATE USER MAPPING FOR public SERVER store OPTIONS (user 'rob');
CREATE FOREIGN TABLE sales (id int, customer_id integer, sale_amount int, created_at timestamp, updated_at timestamp)
 SERVER store OPTIONS (schema_name 'public', table_name 'burrito_sales');

INSERT INTO burrito_sales_archive(id, customer_id, sale_amount, created_at, updated_at)
SELECT id, customer_id, sale_amount, created_at, updated_at
FROM sales order by id;
```

At this point, we should be able to query and interact with the desired table from either database.  Now we can just create a basic view to make things easier for our poor application:  
```
CREATE VIEW total_sales as
SELECT * from burrito_sales
UNION
SELECT * from sales_archive;

EXPLAIN SELECT * FROM total_sales ;
"HashAggregate  (cost=248.19..277.94 rows=2975 width=28)"
"  Group Key: burrito_sales.id, burrito_sales.customer_id, burrito_sales.sale_amount, burrito_sales.created_at, burrito_sales.updated_at"
"  ->  Append  (cost=0.00..211.00 rows=2975 width=28)"
"        ->  Seq Scan on burrito_sales  (cost=0.00..24.00 rows=1400 width=28)"
"        ->  Foreign Scan on sales_archive  (cost=100.00..157.25 rows=1575 width=28)"
```

Viola!  It works. If you look at the cost of the foreign scan, you will see that this operation doesn’t come cheap. That may or may not be important to you at this time and at a minimum, this strategy can at least get your Proof of Concept going while you check out gems like Octopus or additional decorators for your connection string/models.
