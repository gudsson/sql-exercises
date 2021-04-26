# Exercises #

1. ##### Write a query that returns all of the customer names from the orders table.

   ```sql
   SELECT customer_name FROM orders;
   ```

2. ##### Write a query that returns all of the orders that include a Chocolate Shake.

   ```sql
   SELECT * FROM orders
   WHERE drink = 'Chocolate Shake';
   ```

   

3. ##### Write a query that returns the burger, side, and drink for the order with an id of 2.

   ```sql
   SELECT burger, side, drink FROM orders
   WHERE id = 2;
   ```

4. ##### Write a query that returns the name of anyone who ordered Onion Rings.

   ```sql
   SELECT customer_name FROM orders
   WHERE side = 'Onion Rings';
   ```

   

   