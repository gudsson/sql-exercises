# Using Foreign Keys

## Practice Problems

1. Import [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/relational-data-and-joins/foreign-keys/orders_products1.sql) into a new database.

   You may experience some errors while importing this database:

   ```plaintext
   ERROR:  relation "public.orders" does not exist
   ERROR:  relation "public.products" does not exist
   ERROR:  relation "public.orders" does not exist
   ERROR:  relation "public.products" does not exist
   ERROR:  relation "public.orders" does not exist
   ERROR:  sequence "products_id_seq" does not exist
   ERROR:  table "products" does not exist
   ERROR:  sequence "orders_id_seq" does not exist
   ERROR:  table "orders" does not exist
   ```

   You may ignore these errors.

   ```shell
   $ psql -d lesson_3 < order_products1.sql
   ```

   

2. Update the `orders` table so that referential integrity will be preserved for the data between `orders` and `products`.

   ```sql
   ALTER TABLE orders ADD CONSTRAINT orders_product_id_key FOREIGN KEY (product_id) REFERENCES products(id);
   ```

   

3. Use `psql` to insert the data shown in the following table into the database:

   | Quantity | Product    |
   | :------- | :--------- |
   | 10       | small bolt |
   | 25       | small bolt |
   | 15       | large bolt |

   ```sql
   INSERT INTO products (name) VALUES ('small bolt'), ('large bolt');
   
   INSERT INTO orders (product_id, quantity) VALUES (1, 10);
   INSERT INTO orders (product_id, quantity) VALUES (1, 25);
   INSERT INTO orders (product_id, quantity) VALUES (2, 15);
   ```

   

4. Write a SQL statement that returns a result like this:

   ```psql
    quantity |    name
   ----------+------------
          10 | small bolt
          25 | small bolt
          15 | large bolt
   (3 rows)
   ```

   ```sql
   SELECT o.quantity, p.name
   	FROM orders AS o
   	INNER JOIN products AS p
   		ON o.product_id = p.id;
   ```

   

5. Can you insert a row into `orders` without a `product_id`? Write a SQL statement to prove your answer.

   ```sql
   # yes
   INSERT INTO orders (quantity) VALUES (69);
   ```

   

6. Write a SQL statement that will prevent NULL values from being stored in `orders.product_id`. What happens if you execute that statement?

   ```sql
   ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;
   
   # ERROR: column "product_id" of relation "orders" contains null values
   ```

   

7. Make any changes needed to avoid the error message encountered in #6.

   ```sql
   DELETE FROM orders WHERE product_id IS NULL;
   
   ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;
   ```

   

8. Create a new table called `reviews` to store the data shown below. This table should include a primary key and a reference to the `products` table.

   | Product    | Review                  |
   | :--------- | :---------------------- |
   | small bolt | a little small          |
   | small bolt | very round!             |
   | large bolt | could have been smaller |

   ```sql
   CREATE TABLE reviews (
   	id serial PRIMARY KEY,
       product_id integer REFERENCES products(id),
       review text NOT NULL
   );
   ```

   

9. Write SQL statements to insert the data shown in the table in #8.

   ```sql
   INSERT INTO reviews (product_id, review) VALUES (1, 'a little small');
   INSERT INTO reviews (product_id, review) VALUES (1, 'very round!');
   INSERT INTO reviews (product_id, review) VALUES (2, 'could have been smaller');
   ```

   

10. **True** or **false**: A foreign key constraint prevents NULL values from being stored in a column.

    ```
    false
    ```
    
    