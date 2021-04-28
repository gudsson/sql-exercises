# PostgreSQL Data Types

## Practice Problems

1. Describe the difference between the `varchar` and `text` data types.

   ```
   varchar stores characters of text up to the input length specified
   text stores an "unlimited" length of text (long strings)
   ```

   

2. Describe the difference between the `integer`, `decimal`, and `real` data types.

   ```
   integer stores whole numbers between -2147483648 and +2147483647
   decimal stores arbitrary precision numbers (precision dictated by table designer)
   real stores floating-point numbers without arbitrary setting of precision
   ```

   

3. What is the largest value that can be stored in an `integer` column?

   ```
   2147483647
   ```

   

4. Describe the difference between the `timestamp` and `date` data types.

   ```
   timestamp includes date and time
   date just stores the date.
   ```

   

5. Can a time with a time zone be stored in a column of type `timestamp`?

   ```
   No, that requires timestamptz
   ```

   

