# Set Up the Database using \copy

```sql
DROP DATABASE auction;
CREATE DATABASE auction;

CREATE TABLE bidders (
	id serial PRIMARY KEY,
    name text NOT NULL
);

CREATE TABLE items (
	id serial PRIMARY KEY,
    name text NOT NULL,
    initial_price numeric(6,2) NOT NULL CHECK (initial_price BETWEEN 0.01 AND 1000.00),
    sales_price numeric(6,2) CHECK (initial_price BETWEEN 0.01 AND 1000.00)
);

CREATE TABLE bids (
	id serial PRIMARY KEY,
    bidder_id integer NOT NULL REFERENCES bidders (id) ON DELETE CASCADE,
    item_id integer NOT NULL REFERENCES items (id) ON DELETE CASCADE,
    amount numeric(6,2) CHECK (amount BETWEEN 0.01 AND 1000.00)
);

CREATE INDEX ON bids (bidder_id, item_id);


-- Copy data for bidders from the csv file to the bidders table
\copy bidders FROM 'bidders.csv' WITH HEADER CSV;

-- Copy data for items from the csv file to the items table
\copy items FROM 'items.csv' WITH HEADER CSV;

-- Copy data for bids from the csv file to the bids table
\copy bids FROM 'bids.csv' WITH HEADER CSV;
```

# Query From a Virtual Table

```sql
SELECT max(temp.count)
FROM (SELECT bidder_id, count(id)
     FROM bids
     GROUP BY bidder_id) AS temp;
```



# Scalar Subqueries

```sql
SELECT items.name, (SELECT count(item_id) FROM bids
                   WHERE bids.item_id = items.id)
      FROM items;
```

