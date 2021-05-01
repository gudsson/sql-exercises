# Set Up Database

In this set of exercises, we will work with a billing database for a company that provides web hosting services to its customers. The database will contain information about its customers and the services each customer uses. Each customer can have any number of services, and every service can have any number of customers. Thus, there will be a many-to-many (M:M) relationship between the customers and the services. Some customers don't presently have any services, and not every service must be in use by any customers.

Initially, we need to create a `billing` database with a `customers` table and a `services` table. The `customers` table should include the following columns:

- `id` is a unique numeric customer id that auto-increments and serves as a primary key for this table.
- `name` is the customer's name. This value must be present in every record and may contain names of any length.
- `payment_token` is an 8-character string that consists of solely uppercase alphabetic letters. It identifies each customer's payment information with the payment processor the company uses.

```sql
CREATE TABLE customers (
	id serial PRIMARY KEY,
    name text NOT NULL,
    payment_token char(8) NOT NULL UNIQUE CHECK (payment_token ~ '^[A-Z]{8}$')
);
```

The `services` table should include the following columns:

- `id` is a unique numeric service id that auto-increments and serves as a primary key for this table.
- `description` is the service description. This value must be present and may contain any text.
- `price` is the annual service price. It must be present, must be greater than or equal to `0.00`. The data type is `numeric(10, 2)`.

```sql
CREATE TABLE services (
	id serial PRIMARY KEY,
    description text NOT NULL,
    price numeric NOT NULL CHECK (price >= 0.00)
);
```



Once you've created these tables, here is some data that you can enter into them (feel free to enter some data of your own as well):

```plaintext
-- Data for the customers table

id | name          | payment_token
--------------------------------
1  | Pat Johnson   | XHGOAHEQ
2  | Nancy Monreal | JKWQPJKL
3  | Lynn Blake    | KLZXWEEE
4  | Chen Ke-Hua   | KWETYCVX
5  | Scott Lakso   | UUEAPQPS
6  | Jim Pornot    | XKJEYAZA
```

```sql
INSERT INTO customers (name, payment_token) VALUES ('Pat Johnson', 'XHGOAHEQ');
INSERT INTO customers (name, payment_token) VALUES ('Nancy Monreal', 'JKWQPJKL');
INSERT INTO customers (name, payment_token) VALUES ('Lynn Blake', 'KLZXWEEE');
INSERT INTO customers (name, payment_token) VALUES ('Chen Ke-Hua', 'KWETYCVX');
INSERT INTO customers (name, payment_token) VALUES ('Scott Lakso', 'UUEAPQPS');
INSERT INTO customers (name, payment_token) VALUES ('Jim Pornot', 'XKJEYAZA');
```



```plaintext
-- Data for the services table

id | description         | price
---------------------------------
1  | Unix Hosting        | 5.95
2  | DNS                 | 4.95
3  | Whois Registration  | 1.95
4  | High Bandwidth      | 15.00
5  | Business Support    | 250.00
6  | Dedicated Hosting   | 50.00
7  | Bulk Email          | 250.00
8  | One-to-one Training | 999.00
```

```sql
INSERT INTO services (description, price) VALUES ('Unix Hosting', 5.95);
INSERT INTO services (description, price) VALUES ('DNS', 4.95);
INSERT INTO services (description, price) VALUES ('Whois Registration', 1.95);
INSERT INTO services (description, price) VALUES ('High Bandwidth', 15.00);
INSERT INTO services (description, price) VALUES ('Business Support', 250.00);
INSERT INTO services (description, price) VALUES ('Dedicated Hosting', 50.00);
INSERT INTO services (description, price) VALUES ('Bulk Email', 250.00);
INSERT INTO services (description, price) VALUES ('One-to-one Training', 999.00);
```



Once you have entered the data into your tables, create a join table that associates customers with services and vice versa. The join table should have columns for both the services id and the customers id, as well as a primary key named `id` that auto-increments.

The customer id in this table should have the property that deleting the corresponding customer record from the `customers` table will automatically delete all rows from the join table that have that customer_id. Do **not** apply this same property to the service id.

Each combination of customer and service in the table should be unique. In other words, a customer shouldn't be listed as using a particular service more than once.

```sql
CREATE TABLE customers_services (
	id serial PRIMARY KEY,
    service_id integer
    	REFERENCES services(id)
    	NOT NULL,
    customer_id integer
    	REFERENCES customers(id)
    	ON DELETE CASCADE
    	NOT NULL,
    UNIQUE(customer_id, service_id)
);
```

Enter some data in the join table that shows which services each customer uses as follows:

- Pat Johnson uses Unix Hosting, DNS, and Whois Registration
- Nancy Monreal doesn't have any active services
- Lynn Blake uses Unix Hosting, DNS, Whois Registration, High Bandwidth, and Business Support
- Chen Ke-Hua uses Unix Hosting and High Bandwidth
- Scott Lakso uses Unix Hosting, DNS, and Dedicated Hosting
- Jim Pornot uses Unix Hosting, Dedicated Hosting, and Bulk Email

```sql
INSERT INTO customers_services (customer_id, service_id) VALUES (1, 1);
INSERT INTO customers_services (customer_id, service_id) VALUES (1, 2);
INSERT INTO customers_services (customer_id, service_id) VALUES (1, 3);

INSERT INTO customers_services (customer_id, service_id) VALUES (3, 1);
INSERT INTO customers_services (customer_id, service_id) VALUES (3, 2);
INSERT INTO customers_services (customer_id, service_id) VALUES (3, 3);
INSERT INTO customers_services (customer_id, service_id) VALUES (3, 4);
INSERT INTO customers_services (customer_id, service_id) VALUES (3, 5);

INSERT INTO customers_services (customer_id, service_id) VALUES (4, 1);
INSERT INTO customers_services (customer_id, service_id) VALUES (4, 4);

INSERT INTO customers_services (customer_id, service_id) VALUES (5, 1);
INSERT INTO customers_services (customer_id, service_id) VALUES (5, 2);
INSERT INTO customers_services (customer_id, service_id) VALUES (5, 6);

INSERT INTO customers_services (customer_id, service_id) VALUES (6, 1);
INSERT INTO customers_services (customer_id, service_id) VALUES (6, 6);
INSERT INTO customers_services (customer_id, service_id) VALUES (6, 7);
```

# Get Customers With Services

Write a query to retrieve the `customer` data for every customer who currently subscribes to at least one service.

```sql
SELECT *
	FROM customers AS c
	WHERE c.id IN (SELECT customer_id FROM customers_services);
```

# Get Customers With No Services

Write a query to retrieve the `customer` data for every customer who does not currently subscribe to any services.

```sql
SELECT c.*
	FROM customers AS c
	LEFT OUTER JOIN customers_services AS c_S
		ON c.id = c_s.customer_id
	WHERE c_s.customer_id IS NULL;
```

# Get Services With No Customers

Using RIGHT OUTER JOIN, write a query to display a list of all services that are not currently in use. Your output should look like this:

```plaintext
 description
-------------
 One-to-one Training
(1 row)
```

```sql
SELECT s.description
	FROM customers_services AS c_s
	RIGHT OUTER JOIN services AS s
		ON c_s.service_id = s.id
	WHERE c_s.service_id IS NULL;
```

# Services for each Customer

Write a query to display a list of all customer names together with a comma-separated list of the services they use. Your output should look like this:

```plaintext
     name      |                                services
---------------+-------------------------------------------------------------------------
 Pat Johnson   | Unix Hosting, DNS, Whois Registration
 Nancy Monreal |
 Lynn Blake    | DNS, Whois Registration, High Bandwidth, Business Support, Unix Hosting
 Chen Ke-Hua   | High Bandwidth, Unix Hosting
 Scott Lakso   | DNS, Dedicated Hosting, Unix Hosting
 Jim Pornot    | Unix Hosting, Dedicated Hosting, Bulk Email
(6 rows)
```

```sql
SELECT c.name, string_agg(s.description, ', ') AS services
	FROM customers AS c
	LEFT OUTER JOIN customers_services AS c_s
		ON c.id = c_s.customer_id
	LEFT OUTER JOIN services AS s
		ON c_s.service_id = s.id
	GROUP BY c.id
	ORDER BY c.id;
```

# Services With At Least 3 Customers

Write a query that displays the description for every service that is subscribed to by at least 3 customers. Include the customer count for each description in the report. The report should look like this:

```plaintext
 description  | count
--------------+-------
 DNS          |     3
 Unix Hosting |     5
(2 rows)
```

```sql
SELECT s.description, count(s.id) AS count
	FROM services AS s
	INNER JOIN customers_services AS c_s
		ON s.id = c_s.service_id
	GROUP BY s.description
	HAVING count(c_s.customer_id) >= 3;
```

# Total Gross Income

Assuming that everybody in our database has a bill coming due, and that all of them will pay on time, write a query to compute the total gross income we expect to receive.

Answer:

```plaintext
  gross
 --------
 678.50
(1 row)
```

```sql
SELECT sum(s.price) AS gross
	FROM customers_services AS c_s
	INNER JOIN services AS s
		ON s.id = c_s.service_id;
```

# Add New Customer

A new customer, 'John Doe', has signed up with our company. His payment token is 'EYODHLCN'. Initially, he has signed up for UNIX hosting, DNS, and Whois Registration. Create any SQL statement(s) needed to add this record to the database.

```sql
INSERT INTO customers (name, payment_token) VALUES ('John Doe', 'EYODHLCN');
INSERT INTO customers_services (customer_id, service_id)
	VALUES (7, 1),
		   (7, 2),
		   (7, 3);
```

# Hypothetically

The company president is looking to increase revenue. As a prelude to his decision making, he asks for two numbers: the amount of expected income from "big ticket" services (those services that cost more than $100) and the maximum income the company could achieve if it managed to convince all of its customers to select all of the company's big ticket items.

For simplicity, your solution should involve two separate SQL queries: one that reports the current expected income level, and one that reports the hypothetical maximum. The outputs should look like this:

```plaintext
 sum
--------
 500.00
(1 row)
```

```sql
SELECT sum(s.price)
	FROM services AS s
	INNER JOIN customers_services AS c_s
		ON s.id = c_s.service_id
	WHERE s.price > 100.00;
```

```plaintext
   sum
---------
 10493.00
(1 row)
```

```sql
SELECT sum(s.price) * (SELECT count(id) FROM customers) AS sum
	FROM services AS s
	WHERE s.price > 100.00;
```

# Deleting Rows

Write the necessary SQL statements to delete the "Bulk Email" service and customer "Chen Ke-Hua" from the database.

```sql
DELETE FROM customers WHERE name = 'Chen Ke-Hua';
DELETE FROM customers_services
	WHERE service_id = (SELECT id FROM services WHERE description = 'Bulk Email');
DELETE FROM services WHERE description = 'Bulk Email'; 
```

