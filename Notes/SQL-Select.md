# SQL Introduction

## History

- developed by IBM
- Donald Chamberlin & Raymond Boyce
- Originally called SEQUEL but changed to SQL due to trademark issues
- Initially every vendor had its own variant of SQL
- Oracle SQL was not compatible with DB/2 SQL
- This is still true to an extent
  - MySQL & Postgresql have different features and flavors
  - Case sensitivity is a big one!
    - MySQL & SQL - not case sensitive
- Standardization efforts
  - SQL86, SQL89...SQL2016
  - SQL99 is a popular standard
    - Standardized majority of the SQL language
    - Was heavily referred to during the dotCom era
    - MySQL kinda sorta implemented it

## SQL As A Language

- SQL is a declarative programming language
  - You tell the computer what you want, not how to get it
- It’s a functional language!
- Perhaps the most successful functional language in history
- SQL is NOT case sensitive
- Conventions
  - Key words are all caps
  - Tables are often capitalized
  - Columns are usually lower case
    - Sometimes camel case, sometimes underscore separated

# SQL Statements

## SELECT

The `SELECT` statement is the most complex statement in SQL

**Mathematical statements are possible**:

```sql
SELECT
  1 + 1;
```

The result of this select statement is the value 2

**Selecting specific columns `FROM` a specific table**:

```sql
SELECT
  trackid,
  name,
  composer,
  unitprice,
FROM
  tracks;
```

Returns these column values for all rows.

**Select _all_ values from a row**:

```sql
SELECT
  *
FROM
  tracks;
```

You can use the asterisk operator to return all columns.

| Pros | Cons |
| :-- | :-- |
| Shorter | Might bring back unused data |
| Easier to get right | Unclear what columns are available |

## WHERE

- Typically not useful to bring back all the data of a table
- You often want to find a particular piece of data
- The WHERE clause allows you to give predicates that a row must satisfy in order to be included in the results

**Enter the `WHERE` clause**:

```sql
SELECT
  name
FROM
  tracks
WHERE
  milliseconds > 3 * 60 * 1000;
```

Show me the name of all tracks that are longer than 3 minutes.

**General Form**:

```sql
SELECT
  column_list
FROM
  table
WHERE
  search_condition;
```
