# SQL Language

## Practice Problems

1. What kind of programming language is SQL?

   ```
   Special Purpose, Generally Delcarative programming language
   ```

   

2. What are the three sublanguages of SQL?

   ```
   1. Data Defintion Language (DDL)
   2. Data Manipulation Language (DML)
   3. Data Control Language (DCL)
   ```

   

3. Write the following values as quoted string values that could be used in a SQL query.

   ```none
   canoe
   a long road
   weren't
   "No way!"
   ```

   ```sql
   'canoe'
   'a long road'
   'weren''t'
   '"No way!"'
   ```

   

4. What operator is used to concatenate strings?

   ```sql
   ||
   ```

   

5. What function returns a lowercased version of a string? Write a SQL statement using it.

   ```sql
   lower()
   ```

   

6. How does the `psql` console display true and false values?

   ```sql
   t and f
   ```

   

7. The surface area of a sphere is calculated using the formula `A = 4π r2`, where `A` is the surface area and `r` is the radius of the sphere.

   Use SQL to compute the surface area of a sphere with a radius of 26.3cm, truncated to return an integer.

   ```sql
   SELECT trunc(4 * pi() * 26.3 ^ 2);
   ```

   