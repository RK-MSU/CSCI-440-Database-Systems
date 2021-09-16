# SQL - Sub Queries

## Basic Subqueries

A Subquery is a query inside another query

```sql
SELECT
  name
FROM
  tacks
WHERE
  AlbumID = (SELECT AlbumID
             FROM albums
             WHERE Title="Machine Head");
```

Give me the name of all tracks whose `AlbumID` is the same as the `AlbumID` of the album with the title “Machine Head”

**What if we wanted the names of tracks that are on albums whose title starts with “A”?**

```sql
SELECT
  name
FROM
  tacks
WHERE
  AlbumID IN (SELECT AlbumID
             FROM albums
             WHERE Title LIKE "A%");
```

Give me the name all tracks on albums that start with an “A”

## Subqueries & Joins

This subquery example is closely related to a concept of `JOIN`s:

```sql
SELECT
  name
FROM
  tacks
WHERE
  AlbumID IN (SELECT AlbumID
             FROM albums
             WHERE Title LIKE "A%");
```

`JOIN`s correlate tables via Foreign Keys

## Correlated Subqueries

- So far we have looked at subqueries that are independent of the outer query
- The outer query depends on the inner query, but not vice versa
- Is it possible for the inner query to depend on the outer?

**Here is an example**:

```sql
SELECT title
FROM albums
WHERE 10000000 > (
  SELECT sum(bytes)
  FROM tracks
  WHERE tracks.AlbumId = albums.AlbumId);
```

Show me all titles of albums whose tracks size sum to less than 1M bytes

> _Note_: that the outer query albums is referenced in the inner query selecting tracks<br><br>The `sum()` function is used to compute the total number of bytes of all the tracks.

**But there is a catch…**: This approach causes N + 1 queries:

- 1 query to select all album rows
- N queries to select all tracks per album

**How can we get around it?**

- Denormalize the album size
- Move condition into subquery

_Denormalize the data into the album table_:

```sql
--- Denormalized query
SELECT title
FROM albums
WHERE 10000000 > albums.bytes;
```

_Move condition into subquery_:

```sql
--- using a GROUP BY statement
SELECT albums.title
FROM albums
WHERE albums.AlbumId IN 
  (SELECT AlbumId FROM tracks
  GROUP BY AlbumId
  HAVING 10000000 > SUM(tracks.bytes));
```

This eliminates the correlation and makes it a 2 query job

**Where else can we use a correlated subquery?** In the select criterion!

```sql
SELECT title,
  (SELECT sum(bytes)
   FROM tracks
   WHERE tracks.AlbumId = albums.AlbumId)
    AS size
FROM albums
WHERE 10000000 > size;
```

- Note that we can refer to the synthetic column in the where clause
- Pretty advanced stuff...
- Now we can see the size of the album in the results…
- Unfortunately this doesn’t address the performance issues

## Subqueries Summary

A `SELECT` within a `SELECT`

- **Independent Subqueries**
  - Usually involve an `IN` expression
  - Closely related to `JOIN`s
  - Somewhat rare but very useful in real world applications
- **Correlated Subqueries**
  - The inner query refers to the outer queries data
  - Very powerful!
  - Performance issues (N+1) queries
  - Limited use in real world applications because of this
