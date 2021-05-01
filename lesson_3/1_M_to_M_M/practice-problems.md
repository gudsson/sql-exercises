# Converting a 1:M Relationship to a M:M Relationship

## Practice Problems

1. Import [this file](https://raw.githubusercontent.com/launchschool/sql_course_data/master/sql-and-relational-databases/relational-data-and-joins/converting-a-1m-relationship-to-a-mm-relationship/films7.sql) into a database using `psql`.

   ```shell
   $ psql -d 1M_to_MM < films7.sql
   ```

   

2. Write the SQL statement needed to create a join table that will allow a film to have multiple directors, and directors to have multiple films. Include an `id` column in this table, and add foreign key constraints to the other columns.

   ```sql
   CREATE TABLE directors_films (
       id serial PRIMARY KEY,
       director_id integer REFERENCES directors(id)
   	film_id integer REFERENCES films(id)
   );
   ```

   

3. Write the SQL statements needed to insert data into the new join table to represent the existing one-to-many relationships.

   ```sql
   INSERT INTO directors_films (director_id, film_id) VALUES (1, 1);
   INSERT INTO directors_films (director_id, film_id) VALUES (2, 2);
   INSERT INTO directors_films (director_id, film_id) VALUES (3, 3);
   INSERT INTO directors_films (director_id, film_id) VALUES (4, 4);
   INSERT INTO directors_films (director_id, film_id) VALUES (5, 5);
   INSERT INTO directors_films (director_id, film_id) VALUES (6, 6);
   INSERT INTO directors_films (director_id, film_id) VALUES (3, 7);
   INSERT INTO directors_films (director_id, film_id) VALUES (7, 8);
   INSERT INTO directors_films (director_id, film_id) VALUES (8, 9);
   INSERT INTO directors_films (director_id, film_id) VALUES (4, 10);
   ```

   

4. Write a SQL statement to remove any unneeded columns from `films`.

   ```sql
   ALTER TABLE films DROP COLUMN director_id;
   ```

   

5. Write a SQL statement that will return the following result:

   ```psql
              title           |         name
   ---------------------------+----------------------
    12 Angry Men              | Sidney Lumet
    1984                      | Michael Anderson
    Casablanca                | Michael Curtiz
    Die Hard                  | John McTiernan
    Let the Right One In      | Michael Anderson
    The Birdcage              | Mike Nichols
    The Conversation          | Francis Ford Coppola
    The Godfather             | Francis Ford Coppola
    Tinker Tailor Soldier Spy | Tomas Alfredson
    Wayne's World             | Penelope Spheeris
   (10 rows)
   ```

   ```sql
   SELECT f.title, d.name
   	FROM films as f
   	INNER JOIN directors_films AS d_f
   		ON f.id = d_f.film_id
   	INNER JOIN directors AS d
   		ON d.id = d_f.director_id
   	ORDER BY f.title ASC;
   ```

   

6. Write SQL statements to insert data for the following films into the database:

   | Film                   | Year | Genre   | Duration | Directors                      |
   | :--------------------- | :--- | :------ | :------- | :----------------------------- |
   | Fargo                  | 1996 | comedy  | 98       | Joel Coen                      |
   | No Country for Old Men | 2007 | western | 122      | Joel Coen, Ethan Coen          |
   | Sin City               | 2005 | crime   | 124      | Frank Miller, Robert Rodriguez |
   | Spy Kids               | 2001 | scifi   | 88       | Robert Rodriguez               |

   ```sql
   INSERT INTO films (title, year, genre, duration)
   	VALUES ('Fargo', 1996, 'comedy', 98);
   INSERT INTO directors (name) VALUES ('Joel Coen');
   INSERT INTO directors_films (film_id, director_id) VALUES (11, 9);
   	
   INSERT INTO films (title, year, genre, duration)
   	VALUES ('No Country for Old Men', 2007, 'western', 122);
   INSERT INTO directors (name) VALUES ('Ethan Coen');
   INSERT INTO directors_films (film_id, director_id) VALUES (12, 9);
   INSERT INTO directors_films (film_id, director_id) VALUES (12, 10);
   	
   INSERT INTO films (title, year, genre, duration)
   	VALUES ('Sin City', 2005, 'v', 124);
   INSERT INTO directors (name) VALUES ('Frank Miller');
   INSERT INTO directors (name) VALUES ('Robert Rodriguez');
   INSERT INTO directors_films (film_id, director_id) VALUES (13, 11);
   INSERT INTO directors_films (film_id, director_id) VALUES (13, 12);
   
   INSERT INTO films (title, year, genre, duration)
   	VALUES ('Spy Kids', 2001, 'scifi', 88);
   INSERT INTO directors_films (film_id, director_id) VALUES (14, 12);
   	
   ```

   

7. Write a SQL statement that determines how many films each director in the database has directed. Sort the results by number of films (greatest first) and then name (in alphabetical order).

   ```
          director       | films
   ----------------------+-------
    Francis Ford Coppola |     2
    Joel Coen            |     2
    Michael Anderson     |     2
    Robert Rodriguez     |     2
    Ethan Coen           |     1
    Frank Miller         |     1
    John McTiernan       |     1
    Michael Curtiz       |     1
    Mike Nichols         |     1
    Penelope Spheeris    |     1
    Sidney Lumet         |     1
    Tomas Alfredson      |     1
   (12 rows)
   You may assume that every director has at least one film.
   ```

   

   ```sql
   SELECT d.name AS director, count(f.id) AS films
   	FROM directors AS d
   	INNER JOIN directors_films AS d_f
   		ON d.id = d_f.director_id
   	INNER JOIN films AS f
   		ON d_f.film_id = f.id
   	GROUP BY d.name
   	ORDER BY films DESC, director ASC;
   ```

   