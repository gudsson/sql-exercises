# One-to-Many Relationships

## Practice Problems

1. Write a SQL statement to add the following call data to the database:

   | when                | duration | first_name | last_name | number     |
   | :------------------ | :------- | :--------- | :-------- | :--------- |
   | 2016-01-18 14:47:00 | 632      | William    | Swift     | 7204890809 |

   ```sql
   INSERT INTO calls ("when", duration, contact_id) VALUES ('2016-01-18 14:47:00', 632, );
   ```

   

2. Write a SQL statement to retrieve the call times, duration, and first name for all calls **not** made to William Swift.

   Show Solution

3. Write SQL statements to add the following call data to the database:

   | when                | duration | first_name | last_name | number     |
   | :------------------ | :------- | :--------- | :-------- | :--------- |
   | 2016-01-17 11:52:00 | 175      | Merve      | Elk       | 6343511126 |
   | 2016-01-18 21:22:00 | 79       | Sawa       | Fyodorov  | 6125594874 |

   Show Solution

4. Add a constraint to **contacts** that prevents a duplicate value being added in the column `number`.

   Show Solution

5. Write a SQL statement that attempts to insert a duplicate number for a new contact but fails. What error is shown?

   Show Solution

6. Why does "when" need to be quoted in many of the queries in this lesson?

   Show Solution

7. Draw an entity-relationship diagram for the data we've been working with in this assignment.

   Show Solution