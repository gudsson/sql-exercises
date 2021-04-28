# DML, DDL, and DCL Exercises

## DML/DDL/DCL Part 1

SQL consists of 3 different sublanguages. For example, one of these sublanguages is called the Data Control Language (DCL) component of SQL. It is used to control access to a database; it is responsible for defining the rights and roles granted to individual users. Common SQL DCL statements include:

```sql
GRANT
REVOKE
```

Name and define the remaining 2 sublanguages, and give at least 2 examples of each.

### Solution

```
The remaining sublanguages are Data Definition Language (DDL) and Data Manuipulation Language (DML)

Two Examples of DDL Statements:
- CREATE
- ALTER

Two Examples of DML Statements:
- INSERT
- UPDATE
```

## DML/DDL/DCL Part 2

Does the following statement use the Data Definition Language (DDL) or the Data Manipulation Language (DML) component of SQL?

```sql
SELECT column_name FROM my_table;
```

### Solution

```
DML
```

## DML/DDL/DCL Part 3

Does the following statement use the DDL or DML component of SQL?

```sql
CREATE TABLE things (
  id serial PRIMARY KEY,
  item text NOT NULL UNIQUE,
  material text NOT NULL
);
```

### Solution

```
DDL
```

## DML/DDL/DCL Part 4

Does the following statement use the DDL or DML component of SQL?

```sql
ALTER TABLE things
DROP CONSTRAINT things_item_key;
```

### Solution

```
DDL
```

## DML/DDL/DCL Part 5

Does the following statement use the DDL or DML component of SQL?

```sql
INSERT INTO things VALUES (3, 'Scissors', 'Metal');
```

### Solution

```
DML
```

## DML/DDL/DCL Part 6

Does the following statement use the DDL or DML component of SQL?

```sql
UPDATE things
SET material = 'plastic'
WHERE item = 'Cup';
```

### Solution

```
DML
```

## DML/DDL/DCL Part 7

Does the following statement use the DDL, DML, or DCL component of SQL?

```sql
\d things
```

### Solution

```
DDL - X

# Actual answer is none, though it shows the schema of a named table
```

## DML/DDL/DCL Part 8

Does the following statement use the DDL or DML component of SQL?

```sql
DELETE FROM things WHERE item = 'Cup';
```

### Solution

```
DML
```

## DML/DDL/DCL Part 9

Does the following statement use the DDL or DML component of SQL?

```sql
DROP DATABASE xyzzy;
```

### Solution

```
DDL
```

## DML/DDL/DCL Part 10

Does the following statement use the DDL or DML component of SQL?

```sql
CREATE SEQUENCE part_number_sequence;
```

### Solution

```
DDL
```

