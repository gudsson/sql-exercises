## Exercises

1. Connect to the `encyclopedia` database. Write a query to return all of the country names along with their appropriate continent names.

   ```sql
   SELECT countries.name, continents.continent_name
   	FROM countries
   	INNER JOIN continents
   	ON (continents.id = countries.continent_id);
   ```

   

2. Write a query to return all of the names and capitals of the European countries.

   ```sql
   SELECT countries.name, countries.capital
   	FROM countries
   	INNER JOIN continents
   	ON (continents.id = countries.continent_id)
   	WHERE continents.continent_name = 'Europe';
   ```

   

3. Write a query to return the first name of any singer who had an album released under the Warner Bros label.

   ```sql
   SELECT DISTINCT celebrities.first_name
   	FROM celebrities
   	INNER JOIN albums
   	ON (celebrities.id = albums.singer_id)
   	WHERE record_label LIKE '%Warner Bros%';
   ```

   

4. Write a query to return the first name and last name of any singer who released an album in the 80s and who is still living, along with the names of the album that was released and the release date. Order the results by the singer's age (youngest first).

   ```sql
   SELECT celebrities.first_name, celebrities.last_name, albums.album_name, albums.release_date, celebrities.date_of_birth
   	FROM celebrities
   	INNER JOIN albums
   	ON (celebrities.id = albums.singer_id)
   	WHERE date_part('year', albums.release_date) >= 1980
   	AND date_part('year', albums.release_date) < 1990
   	AND celebrities.deceased = false
   	ORDER BY celebrities.date_of_birth DESC;
   ```

   

5. Write a query to return the first name and last name of any singer without an associated album entry.

   ```sql
   SELECT celebrities.first_name, celebrities.last_name
   	FROM celebrities
   	LEFT JOIN albums
   	ON (celebrities.id = albums.singer_id)
   	WHERE celebrities.occupation LIKE '%Singer%'
   	AND albums.id IS NULL;
   ```

   

6. Rewrite the query for the last question as a sub-query.

   ```sql
   SELECT celebrities.first_name, celebrities.last_name
   	FROM celebrities
   	WHERE celebrities.occupation LIKE '%Singer%'
   	AND celebrities.id NOT IN (SELECT albums.singer_id FROM albums);
   ```

   

7. Connect to the `ls_burger` database. Return a list of all orders and their associated product items.

   ```sql
   SELECT orders.id, orders.customer_id, orders.order_status, products.product_name
   	FROM orders
   	INNER JOIN order_items ON (orders.id = order_items.order_id)
   	INNER JOIN products ON (order_items.product_id = products.id);
   	
   ```

   

8. Return the id of any order that includes Fries. Use table aliasing in your query.

   ```sql
   SELECT DISTINCT o.id AS "Orders Containing Fries"
   	FROM orders AS o
   	INNER JOIN order_items AS oi ON (o.id = oi.order_id)
   	INNER JOIN products AS p ON (oi.product_id = p.id)
   	WHERE p.product_name = 'Fries';
   ```

   

9. Build on the query from the previous question to return the name of any customer who ordered fries. Return this in a column called 'Customers who like Fries'. Don't repeat the same customer name more than once in the results.

   ```sql
   SELECT DISTINCT c.customer_name AS "Customers who like Fries"
   	FROM orders AS o
   	INNER JOIN customers AS c ON (o.customer_id = c.id)
   	INNER JOIN order_items AS oi ON (o.id = oi.order_id)
   	INNER JOIN products AS p ON (oi.product_id = p.id)
   	WHERE p.product_name = 'Fries';
   ```

   

10. Write a query to return the total cost of Natasha O'Shea's orders.

    ```sql
    SELECT sum(p.product_cost) AS "Total Cost of Natasha's Orders"
    	FROM orders AS o
    	INNER JOIN customers AS c ON (o.customer_id = c.id)
    	INNER JOIN order_items AS oi ON (o.id = oi.order_id)
    	INNER JOIN products AS p ON (oi.product_id = p.id)
    	WHERE c.customer_name = 'Natasha O''Shea';
    
        #  Total Cost of Natasha's Orders
        # --------------------------------
        #                           23.60
    ```

    

11. Write a query to return the name of every product included in an order alongside the number of times it has been ordered. Sort the results by product name, ascending.

    ```sql
    SELECT p.product_name AS "Item", count(p.product_name) AS "Times Ordered"
    	FROM orders AS o
    	INNER JOIN customers AS c ON (o.customer_id = c.id)
    	INNER JOIN order_items AS oi ON (o.id = oi.order_id)
    	RIGHT JOIN products AS p ON (oi.product_id = p.id)
    	GROUP BY p.product_name
    	ORDER BY p.product_name ASC;
    ```

    