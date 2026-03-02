# Aggregate Functions 
>[!Aggregate Functions]
>A function that *summarizes data from multiple rows into a single result*. 
>- Ex) calculates total number of members, the average age, or the total order amount

Representative Aggregate Functions

|Function|Description|Example Use Case|
|---|---|---|
|**`COUNT()`**|Counts the number of rows.|Total members, number of product types, etc.|
|**`SUM()`**|The total sum of numbers.|Total payment amount, total order quantity, etc.|
|**`AVG()`**|The average value.|Average age, average purchase amount, etc.|
|**`MAX()`**|The maximum value.|Highest price, most recent date, etc.|
|**`MIN()`**|The minimum value.|Lowest age, earliest sign-up date, etc.|
Example
```sql
-- 전체 회원 수 조회
SELECT COUNT(*) FROM members;

-- 평균 나이 조회
SELECT AVG(age) FROM members;

-- 가장 어린 회원의 나이
SELECT MIN(age) FROM members;
```

# `GROUP BY`
- **`GROUP BY`** is a clause that *groups rows based on a specific column*, allowing aggregate functions to be applied to each group.
```sql
-- Query the number of members for each age
SELECT
  age,
  COUNT(*) AS count_by_age
FROM
  members
GROUP BY
  age;
```
- It groups rows from the `members` table that have the same `age` value.
- It then counts the number of people within each group (by age).

Example result

|age|cnt|
|---|---|
|14|1|
|18|1|
|19|1|
|22|2|
|33|1|
## `GROUP BY` + Multiple Aggregate Functions
```sql
SELECT
  age,
  COUNT(*) AS total_members,
  AVG(age) AS avg_age,
  MAX(registered_at) AS latest_reg,
  MIN(registered_at) AS earliest_reg
FROM
  members
GROUP BY
  age;
```
- You only need to write **`GROUP BY`** once, and you can use multiple aggregate functions at the same time.
- Using **`MAX()`** and **`MIN()`** together allows you to easily check date ranges or numerical ranges.
- **`GROUP BY age`**
	- finds all members who have the same age and puts them into separate groups. 
	- Ex) a pile for all the 25-year-olds, another pile for all the 26-year-olds, etc.
- **`SELECT ...`**: Finally, it calculates the following information for **each** of those age piles:
	- **`age`**: The age that defines the group (e.g., 25).
	- **`COUNT(*)`**: Counts how many members are in that group.
	- **`AVG(age)`**: Calculates the average age of the group (which will simply be the group's age, like 25).
	- **`MAX(registered_at)`**: Finds the most recent (`latest`) registration date from all the members within that group.
	- **`MIN(registered_at)`**: Finds the earliest registration date from all the members within that group.

# `GROUP BY` + `ORDER BY`
- sorts the number of members per age group in descending order, showing the most populous age group first.

```sql
SELECT
  age,
  COUNT(*) AS cnt
FROM
  members
GROUP BY
  age
ORDER BY
  cnt DESC;
```

- The **`ORDER BY`** clause is used to sort the results of the aggregation.
- You can sort based on the aggregated column (e.g., `cnt`) or the grouping column (e.g., `age`).

# `HAVING`
This query finds a list of member IDs that have placed 2 or more orders.
```sql
SELECT
  member_id,
  COUNT(*) AS order_count
FROM
  orders
GROUP BY
  member_id
HAVING
  COUNT(*) >= 2;
```
- **`WHERE`** filters rows **before** aggregation, while **`HAVING`** filters groups **after** aggregation.
- In practice, `HAVING` is often used when you need to filter based on statistics.
- `WHERE vs HAVING`
	- `WHERE` is more efficient because it **narrows down the scope first, and then the aggregation happens.**
- When You _Must_ Use `HAVING`
	- when your filter condition depends on the **result of an aggregate function**
	- ex) The condition "have placed 2 or more orders" (`COUNT(*) >= 2`) can only be checked _after_ the orders have been counted for each member.
# Tip
- Unlike a normal `SELECT`, when you use **`GROUP BY`**, every column in the `SELECT` list must either be an aggregate function (like `COUNT()`) or one of the columns you are grouping by.
- Remember the order: **`WHERE`** filters data _before_ grouping, and **`HAVING`** filters the final grouped results _after_ aggregation.
- `COUNT(*)` and `COUNT(column)` work differently. `COUNT(column)` excludes `NULL` values, while `COUNT(*)` counts all rows.
- If the `GROUP BY` column contains `NULL` values, `NULL`s are treated as their own single group.
- When applying `GROUP BY` after a `JOIN`, be careful to avoid unintended duplicates in the join result that could skew your aggregate calculations.