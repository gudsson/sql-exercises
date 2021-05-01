

# Many-to-Many Relationships

# Practice Problems

The following assignments assume you have imported [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/relational-data-and-joins/many-to-many-relationships/many_to_many.sql) into a database.

1. Write a SQL statement that will return the following result:

   ```psql
    id |     author      |           categories
   ----+-----------------+--------------------------------
     1 | Charles Dickens | Fiction, Classics
     2 | J. K. Rowling   | Fiction, Fantasy
     3 | Walter Isaacson | Nonfiction, Biography, Physics
   (3 rows)
   ```

   ```sql
   SELECT b.id, b.author, string_agg(c.name, ', ')
   	FROM books AS b
   	INNER JOIN books_categories AS b_c
   		ON b.id = b_c.book_id
   	INNER JOIN categories AS c
   		ON b_c.category_id = c.id
   	GROUP BY b.id
   	ORDER BY b.id ASC;
   ```

   

2. Write SQL statements to insert the following new books into the database. What do you need to do to ensure this data fits in the database?

   | Author                        | Title                                      | Categories                               |
   | :---------------------------- | :----------------------------------------- | :--------------------------------------- |
   | Lynn Sherr                    | Sally Ride: America's First Woman in Space | Biography, Nonfiction, Space Exploration |
   | Charlotte Brontë              | Jane Eyre                                  | Fiction, Classics                        |
   | Meeru Dhalwala and Vikram Vij | Vij's: Elegant and Inspired Indian Cuisine | Cookbook, Nonfiction, South Asia         |

   ```sql
   ALTER TABLE books ALTER COLUMN title TYPE text;
   
   INSERT INTO books (title, author) VALUES ('Sally Ride: America''s First Woman in Space', 'Lynn Sherr');
   INSERT INTO categories (name) VALUES ('Space Exploration');
   INSERT INTO books_categories (book_id, category_id) VALUES (4, 1);
   INSERT INTO books_categories (book_id, category_id) VALUES (4, 5);
   INSERT INTO books_categories (book_id, category_id) VALUES (4, 7);
   
   INSERT INTO books (title, author) VALUES ('Jane Eyre', 'Charlotte Brontë');
   INSERT INTO books_categories (book_id, category_id) VALUES (5, 2);
   INSERT INTO books_categories (book_id, category_id) VALUES (5, 4);
   
   INSERT INTO books (title, author) VALUES ('Vij''s: Elegant and Inspired Indian Cuisine', 'Meeru Dhalwala and Vikram Vij');
   INSERT INTO categories (name) VALUES ('Cookbook');
   INSERT INTO categories (name) VALUES ('South Asia');
   INSERT INTO books_categories (book_id, category_id) VALUES (6, 8);
   INSERT INTO books_categories (book_id, category_id) VALUES (6, 9);
   INSERT INTO books_categories (book_id, category_id) VALUES (6, 1);
   ```

   

3. <span style="background-color: yellow">Write a SQL statement to add a uniqueness constraint on the combination of columns `book_id` and `category_id` of the `books_categories` table. This constraint should be a table constraint; so, it should check for uniqueness on the combination of `book_id` and `category_id` across all rows of the `books_categories` table.</span>

   ```sql
   ALTER TABLE books_categories ADD UNIQUE (book_id, category_id);
   ```

   

4. Write a SQL statement that will return the following result:

   ```psql
         name        | book_count |                                 book_titles
   ------------------+------------+------------------------------------------------------
   Biography         |          2 | Einstein: His Life and Universe, Sally Ride:         America's First Woman in Space
   Classics          |          2 | A Tale of Two Cities, Jane Eyre
   Cookbook          |          1 | Vij's: Elegant and Inspired Indian Cuisine
   Fantasy           |          1 | Harry Potter
   Fiction           |          3 | Jane Eyre, Harry Potter, A Tale of Two Cities
   Nonfiction        |          3 | Sally Ride: America's First Woman in Space, Einstein: His Life and Universe, Vij's: Elegant and Inspired Indian Cuisine
   Physics           |          1 | Einstein: His Life and Universe
   South Asia        |          1 | Vij's: Elegant and Inspired Indian Cuisine
   Space Exploration |          1 | Sally Ride: America's First Woman in Space
   ```

   ```sql
   SELECT c.name, count(b.id), string_agg(b.title, ', ')
   	FROM categories AS c
   	INNER JOIN books_categories AS b_c
   		ON c.id = b_c.category_id
   	INNER JOIN books AS b
   		ON b_c.book_id = b.id
   	GROUP BY c.name ORDER BY c.name;
   ```

   