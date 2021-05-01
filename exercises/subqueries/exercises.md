# Subqueries and More

# Set Up the Database using \copy

This set of exercises will focus on an auction. Create a new database called `auction`. In this database there will be three tables, `bidders`, `items`, and `bids`.

After creating the database, set up the 3 tables using the following specifications:

##### bidders

- `id` of type SERIAL: this should be a primary key
- `name` of type text: this should be `NOT NULL`

```sql
CREATE TABLE bidders (
	id serial PRIMARY KEY,
    name text NOT NULL
);
```



##### items

- `id` of type SERIAL: this should be a primary key
- `name` of type text: this should be `NOT NULL`
- `initial_price` and `sales_price`: These two columns should both be of type numeric. Each column should be able to hold a number as high as 1000 dollars with 2 decimal points of precision.
- The `initial_price` represents the starting price of an item when it is first put up for auction. This column should never be NULL.
- The `sales_price` represents the final price at which the item was sold. This column may be NULL, as it is possible to have an item that was never sold off.

```sql
CREATE TABLE items (
	id serial PRIMARY KEY,
    name text NOT NULL,
    initial_price numeric(6,2) NOT NULL CHECK (initial_price BETWEEN 0.01 AND 1000.00),
    sales_price numeric(6,2) CHECK (sales_price BETWEEN 0.01 AND 1000.00)
);
```



##### bids

- `id` of type SERIAL: this should be a primary key
- `bidder_id`, `item_id`: These will be of type integer and should not be NULL. This table connects a bidder with an item and each row represents an individual bid. There should never be a row that has `bidder_id` or `item_id` unknown or NULL. Nor should there ever be a bid that references a nonexistent item or bidder. If the item or bidder associated with a bid is removed, that bid should also be removed from the database.
- Create your `bids` table so that both `bidder_id` and `item_id` together form a composite index for faster lookup.
- amount - The amount of money placed for each individual bid by a bidder. This column should be of the same type as `items.initial_price` and have the same constraints.

```sql
CREATE TABLE bids (
	id serial PRIMARY KEY,
    bidder_id integer NOT NULL REFERENCES bidders(id) ON DELETE CASCADE,
    item_id integer NOT NULL REFERENCES bidders(id) ON DELETE CASCADE,
    amount numeric(6,2) NOT NULL CHECK (amount BETWEEN 0.01 AND 1000.00)
);
```



Finally, use the `\copy` meta-command to import the below files into your `auction` database. You'll have to create these files yourself before you can import them with `\copy`.

**bidders.csv**

```plaintext
id, name
1,Alison Walker
2,James Quinn
3,Taylor Williams
4,Alexis Jones
5,Gwen Miller
6,Alan Parker
7,Sam Carter
```

**items.csv**

```plaintext
id, name, initial_price, sales_price
1,Video Game, 39.99, 70.87
2,Outdoor Grill, 51.00, 83.25
3,Painting, 100.00, 250.00
4,Tent, 220.00, 300.00
5,Vase, 20.00, 42.00
6,Television, 550.00,
```

**bids.csv**

```plaintext
id, bidder_id, item_id, amount
1,1, 1, 40.00
2,3, 1, 52.00
3,1, 1, 53.00
4,3, 1, 70.87
5,5, 2, 83.25
6,2, 3, 110.00
7,4, 3, 140.00
8,2, 3, 150.00
9,6, 3, 175.00
10,4, 3, 185.00
11,2, 3, 200.00
12,6, 3, 225.00
13,4, 3, 250.00
14,1, 4, 222.00
15,2, 4, 262.00
16,1, 4, 290.00
17,1, 4, 300.00
18,2, 5, 21.72
19,6, 5, 23.00
20,2, 5, 25.00
21,6, 5, 30.00
22,2, 5, 32.00
23,6, 5, 33.00
24,2, 5, 38.00
25,6, 5, 40.00
26,2, 5, 42.00
```

If you are using AWS Cloud9, and running Postgres as the `postgres` user rather than the local user, you may see a message like the following when you try to run `\copy`:

```psql
bidders.csv: No such file or directory
```

This will likely be because the `postgres` user doesn't have read permissions within the local environment. There's a couple of different ways you can fix this, but our suggestion is to run Postgres as the local user (which should have read permissions) rather than the `postgres` user. Refer back to [this section](https://launchschool.com/books/sql/read/preparations#installingpostgresql) of the Introduction to SQL book for more information about creating such a user if you haven't already done so.

# Conditional Subqueries: IN

Write a SQL query that shows all items that have had bids put on them. Use the logical operator `IN` for this exercise, as well as a subquery.

Here is the expected output:

```psql
 Bid on Items
---------------
 Video Game
 Outdoor Grill
 Painting
 Tent
 Vase
(5 rows)
```

```sql
SELECT name AS "Bid on Items"
	FROM items
	WHERE id IN (SELECT DISTINCT item_id FROM bids);
```

# Conditional Subqueries: NOT IN

Write a SQL query that shows all items that have not had bids put on them. Use the logical operator `NOT IN` for this exercise, as well as a subquery.

Here is the expected output:

```psql
 Not Bid On
------------
 Television
(1 row)
```

```sql
SELECT name AS "Bid on Items"
	FROM items
	WHERE items.id NOT IN (SELECT DISTINCT item_id FROM bids);
```

# Conditional Subqueries: EXISTS

Write a `SELECT` query that returns a list of names of everyone who has bid in the auction. While it is possible (and perhaps easier) to do this with a `JOIN` clause, we're going to do things differently: use a subquery with the `EXISTS` clause instead. Here is the expected output:

```psql
      name
-----------------
 Alison Walker
 James Quinn
 Taylor Williams
 Alexis Jones
 Gwen Miller
 Alan Parker
(6 rows)
```

```sql
SELECT name 
	FROM bidders
		WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
```

# Query From a Virtual Table

For this exercise, we'll make a slight departure from *how* we've been using subqueries. We have so far used subqueries to filter our results using a WHERE clause. In this exercise, we will build that filtering into the table that we will query. Write an SQL query that finds the largest number of bids from an individual bidder.

For this exercise, you must use a subquery to generate a result table (or virtual table), and then query that table for the largest number of bids.

Your output should look like this:

```psql
  max
------
    9
(1 row)
```

```sql
SELECT max(count) FROM (SELECT count(bidder_id) FROM bids GROUP BY bidder_id) as virtual_table;
```

# Scalar Subqueries

For this exercise, use a scalar subquery to determine the number of bids on each item. The entire query should return a table that has the `name` of each item along with the number of bids on an item.

Here is the expected output:

```psql
    name      | count
--------------+-------
Video Game    |     4
Outdoor Grill |     1
Painting      |     8
Tent          |     4
Vase          |     9
Television    |     0
(6 rows)
```

```sql
SELECT name, (SELECT count(item_id) FROM bids WHERE items.id = bids.item_id)
	FROM items;
```

# Row Comparison

We want to check that a given item is in our database. There is one problem though: we have all of the data for the item, but we don't know the `id` number. Write an SQL query that will display the `id` for the item that matches all of the data that we know, but does not use the `AND` keyword. Here is the data we know:

```
'Painting', 100.00, 250.00
```

```sql
SELECT id
	FROM items
		WHERE ROW('Painting', 100.00, 250.00) = ROW(name, initial_price, sales_price);
```

# EXPLAIN

For this exercise, let's explore the `EXPLAIN` PostgreSQL statement. It's a very useful SQL statement that lets us analyze the efficiency of our SQL statements. More specifically, use `EXPLAIN` to check the efficiency of the query statement we used in the exercise on `EXISTS`:

```psql
SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
```

First use just `EXPLAIN`, then include the `ANALYZE` option as well. For your answer, list any SQL statements you used, along with the output you get back, and your thoughts on what is happening in both cases.

```sql
EXPLAIN SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
# Hash join (cost=33.38..66.47 rows=635 width=32)
#  Hash Cond: (bidders.id = bids.bidder_id)
#  ->  Seq Scan on bidders (cost=0.00..22.70 rows=1270 width=36)
#  ->  Hash (cost=30.88..30.88 rows=200 width=4)
#    ->  HashAggregate (cost=28.88..30.88 rows=200 width=4) 
#          Group Key: bids.bidder_id
#          -> Seq Scan on bids (cost=0.00..25.10 rows=1510 width=4)

EXPLAIN ANALYZE SELECT name FROM bidders
WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
# Hash join (cost=33.38..66.47 rows=635 width=32) (actual time=0.029..0.033 row=6 loops=1)
#  Hash Cond: (bidders.id = bids.bidder_id)
#  ->  Seq Scan on bidders (cost=0.00..22.70 rows=1270 width=36) (actual time=0.003..0.007 rows=7 loops=1)
#  ->  Hash (cost=30.88..30.88 rows=200 width=4) (actual time=0.019..0.020 rows=6 loops=1)
#    ->  HashAggregate (cost=28.88..30.88 rows=200 width=4) (actual time=0.016..0.017 rows=6 loops=1)
#          Group Key: bids.bidder_id
#          -> Seq Scan on bids (cost=0.00..25.10 rows=1510 width=4) (actual time=0.004..0.006 rows=26 loops=1)
# Planning Time: 0.108 ms
# Execution Time: 0.058 ms
# (11 rows)
```

# Comparing SQL Statements

In this exercise, we'll use `EXPLAIN ANALYZE` to compare the efficiency of two SQL statements. These two statements are actually from the "Query From a Virtual Table" exercise in this set. In that exercise, we stated that our subquery-based solution:

```psql
SELECT MAX(bid_counts.count) FROM
  (SELECT COUNT(bidder_id) FROM bids GROUP BY bidder_id) AS bid_counts;
```

was actually faster than the simpler equivalent without subqueries:

```psql
SELECT COUNT(bidder_id) AS max_bid FROM bids
  GROUP BY bidder_id
  ORDER BY max_bid DESC
  LIMIT 1;
```

In this exercise, we will demonstrate this fact.

Run `EXPLAIN ANALYZE` on the two statements above. Compare the planning time, execution time, and the total time required to run these two statements. Also compare the total "costs". Which statement is more efficient and why?

Approach/Algorithm

These assignments and PostgreSQL documents should prove useful for this exercise:

- [Assignment on EXPLAIN](https://launchschool.com/lessons/e752508c/assignments/87715c5f)
- [Documentation on SELECT](https://www.postgresql.org/docs/current/static/sql-select.html)
- [Documentation about using EXPLAIN](https://www.postgresql.org/docs/9.6/static/using-explain.html)

