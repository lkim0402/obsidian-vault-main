# Intro
> How to query for data inside one table
- spreadsheets to dbs
	- scale
	- frequency
		- being able to update data more frequently
	- speed
		- look up some info
- A database
	- A collection of data organized for creating, reading, updating, and deleting
- Database Management System
	- Software via which you interact with a database
		- maybe a GUI or a language
	- MySql, Oracle, PostgreSQL, SQLite, etc
	- We will use SQLite
- SQL
	- Structured Query Language
	- A language via which you can create, read, update, and delete data in a db
- Query
	- Writing queries - trying to ask questions
		- what is the most liked post in instagram?
		- is our number of daily users growing or shrinking
```shell
# Open a db
sqlite3 longlist.db
# quit
.quit
```

# Keywords
```sql
-- SELECT 
SELECT * FROM "longlist";
SELECT "title" FROM "longlist";
SELECT "title", "author" FROM "longlist";

-- LIMIT
SELECT "title" FROM "longlist" LIMIT 10;

-- WHERE
SELECT "title" FROM "longlist" WHERE "year" = 2023;
SELECT "title" FROM "longlist" WHERE "year" = 2023 LIMIT 5;
SELECT "title","format" FROM "longlist" WHERE "format" != 'hardcover' LIMIT 5;

-- NOT
SELECT "title","format" FROM "longlist" WHERE NOT "format" = 'hardcover' LIMIT 5;

-- AND, OR, ()
SELECT "title","format" FROM "longlist" WHERE "year" = 2023 AND "format" = 'hardcover' LIMIT 5;
SELECT "title","format" FROM "longlist" WHERE ("year" = 2023 OR "year" = 2024)
	AND "format" = 'hardcover' LIMIT 5;
	
-- NULL
SELECT "title", "translator" FROM "longlist" WHERE "translator" IS NULL;

-- LIKE
SELECt "title" FROM "longlist" WHERE "title" LIKE '%love%';
SELECT "title" FROM "longlist" WHERE "title" LIKE 'the%';
SELECT "title" FROM "longlist" WHERE "title" LIKE 'P_re';

-- >, < >=, <=, BETWEEN ... AND ...
SELECT "title", "year" FROM "longlist" WHERE "year" >= 2019 AND "year" <= 2022;
SELECT "title", "year" FROM "longlist" WHERE "year" BETWEEN 2019 AND 2022;

-- ORDER BY
SELECT "title","rating" FROM "longlist" ORDER BY "rating" LIMIT 10;
SELECT "title","rating" FROM "longlist" ORDER BY "rating" DESC LIMIT 10;
SELECT "title", "rating" FROM "longlist" ORDER BY "rating" DESC, "votes" DESC LIMIT 10; -- 2 conds for tie
```
- UPPERCASE for keywords for style 
	- to differentiate, to make it clear that this is a SQL keyword
- `SELECT`
	- get some rows
	- SQL identifiers - the double quotes around table/column names, style convention (single quotes for string names)
- `LIMIT`
	- limiting number of items
- `WHERE`
	- lets you filter rows
	- `=, !=, <>`
		- `<>` is just `!=`
- `NOT`
	- negate a condition using `NOT`
- `AND`, `OR`, `()`
	- for chaining conditions
- `NULL`
	- value doesn't exist
	- `IS NULL`, `IS NOT NULL`
- `LIKE`
	- for pattern matching, matching some string in my database, case insensitive
	- Becomes powerful with operators: `%`, `_`
		- `%` - can match any character around a string I give it
			- `%love%` - any string can go before and after "love" as long as love is somewhere in there
			- `%love` - any string can only go before "love"
			- `the%` - any string can only go after "the"
				- This might give titles with words like "there"
				- so if you just want "the", then you can do `'the %'` to make this better designed
			- `the%of%`
				- can use for multiple words
		- `_` - can match any single character that I pass in with a string
			- `P_re`
- Operators
	- `>, < >=, <=`
	- We can use this to build ranges in our queries
	- `BETWEEN....AND...`
- `ORDER BY`
	- order by some column, default is ascending (least to greatest)
	- `ORDER BY ... ASC` (default)
	- `ORDER BY ... DESC`
	- Breaking ties -> put 2 conditions

## Aggregate functions
- gives 1 number based on the values in those rows
```sql
-- AVG
SELECT AVG("rating") FROM "longlist";

-- ROUND
SELECT ROUND(AVG("rating"), 2) FROM "longlist";

-- AS
SELECT ROUND(AVG("rating"), 2) AS "average rating" FROM longlist;

-- MIN/MAX
SELECT MAX("rating") FROM "longlist";
SELECT MIN("rating") FROM "longlist";

-- SUM
SELECT SUM("rating") FROM "longlist";

-- COUNT
SELECT COUNT(*) FROM "longlist";
SELECT COUNT("translator") FROM "longlist"; -- some values can be NULL

-- DISTINCT
SELECT DISTINCT "publisher" FROM "longlist";
SELECT COUNT(DISTINCT "publisher") FROM "longlist";
```
- `AVG`
	- average of some column
- `ROUND`
	- round value
- `AS`
	- renames column
- `MIN`/ `MAX`
- `SUM`
- `COUNT`
	- number of rows, counts the rows that are NOT `NULL`
	- counts duplicates
- `DISTINCT`

# Practice
- Write a single SQL query to list the first and last names of all players of above average height, sorted tallest to shortest, then by first and last name.
```sql
SELECT first_name, last_name FROM players WHERE height > (SELECT AVG(height) FROM players) ORDER BY height DESC, first_name, last_name;
```
- `WHERE height > (SELECT AVG(height) FROM players) `
	- You can use an aggregate function to compare in the `WHERE` keyword!

# Order Of Writing / Execution
![](https://www.thedataschool.co.uk/content/images/2024/09/image-45.png)

