# More Constraints

1. Import [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/schema-data-and-sql/more-constraints/films2.sql) into a PostgreSQL database.

   ```sql
   $ psql -d lesson_2 < films2.sql
   ```

   

2. Modify all of the columns to be `NOT NULL`.

   ```sql
   ALTER TABLE films ALTER COLUMN title SET NOT NULL;
   ALTER TABLE films ALTER COLUMN year SET NOT NULL;
   ALTER TABLE films ALTER COLUMN genre SET NOT NULL;
   ALTER TABLE films ALTER COLUMN director SET NOT NULL;
   ALTER TABLE films ALTER COLUMN duration SET NOT NULL;
   ```

   

3. How does modifying a column to be `NOT NULL` affect how it is displayed by `\d films`?

   ```
   column marked 'not null' in nullable column
   ```

   

4. Add a constraint to the table **films** that ensures that all films have a unique title.

   ```sql
   ALTER TABLE films ADD CONSTRAINT unique_title UNIQUE (title);
   ```

   

5. How is the constraint added in #4 displayed by `\d films`?

   ```
   "unique_title" is added as an index.
   ```

   

6. Write a SQL statement to remove the constraint added in #4.

   ```sql
   ALTER TABLE films DROP CONSTRAINT unique_title;
   ```

   

7. Add a constraint to **films** that requires all rows to have a value for `title` that is at least 1 character long.

   ```sql
   ALTER TABLE films ADD CONSTRAINT min_title_length CHECK (length(title) > 0);
   ```

   

8. What error is shown if the constraint created in #7 is violated? Write a SQL `INSERT` statement that demonstrates this.

   ```sql
   INSERT INTO films VALUES ('', 2000, 'Comedy', 'Me', 69);
   
   # ERROR: new row for relation "films" violates check constraint "min_title_length"
   # DETAIL: Failing row contains (, 2000, Comedy, Me, 69).
   ```

   

9. How is the constraint added in #7 displayed by `\d films`?

   ```
   Check constaints:
   	"min_title_length" CHECK (length(title::text) > 0)
   ```

   

10. Write a SQL statement to remove the constraint added in #7.

    ```sql
    ALTER TABLE films
    	DROP CONSTRAINT min_title_length;
    ```

    

11. Add a constraint to the table **films** that ensures that all films have a year between 1900 and 2100.

    ```sql
    ALTER TABLE films
    	ADD CONSTRAINT valid_years CHECK (year >= 1900 AND year <= 2100);
    ```

    

12. How is the constraint added in #11 displayed by `\d films`?

    ```
    Check constraints:
        "year_range" CHECK (year >= 1900 AND year <= 2100)
    ```

    

13. Add a constraint to **films** that requires all rows to have a value for `director` that is at least 3 characters long and contains at least one space character (``).

    ```sql
    ALTER TABLE films ADD CONSTRAINT director_name CHECK (length(director) > 2 AND position(' ' in director) <> 0);
    ```

    

14. How does the constraint created in #13 appear in `\d films`?

    ```
    Check constraints:
        "director_name" CHECK (length(director::text) > 2 AND "position"(director::text, ' '::text) <> 0)
    ```

    

15. Write an `UPDATE` statement that attempts to change the director for "Die Hard" to "Johnny". Show the error that occurs when this statement is executed.

    ```sql
    UPDATE films SET director = 'Johnny' WHERE title = 'Die Hard';
    
    #Gives Error:
    
    # ERROR: new row for relation "films" violates check constaint "director_name"
    # DETAIL: Failing row contains (Die Hard, 1988, action, Johnny, 132)
    ```

    

16. List three ways to use the schema to restrict what values can be stored in a column.

    ```
    1. Use `CHECK` constraints
    2. Use `NOT NULL` constraints
    3. Choose data type
    ```

    

17. Is it possible to define a default value for a column that will be considered invalid by a constraint? Create a table that tests this.

    ```sql
    ALTER TABLE films ADD COLUMN rating int DEFAULT 0;
    ALTER TABLE films ADD CONSTRAINT min_rating CHECK (raitng > 0);
    DELETE FROM films;
    INSERT INTO films VALUES ('Die Hard', 1988, 'action', 'John McTiernan', 132, DEFAULT);
    
    # Can do if table is empty
    ```

    

18. How can you see a list of all of the constraints on a table?

    ```sql
    \d table_name # in the postgres console
    ```

    