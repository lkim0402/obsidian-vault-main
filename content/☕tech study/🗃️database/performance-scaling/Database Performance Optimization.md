# The Problem: Slow Queries
- The more conditions you have in a `WHERE` clause, the more it can slow things down. 
	- [[DML (Data Manipulation Language)#`WHERE`|WHERE clause in DML]]
- When we talk about performance, the most important operation is almost always the **read** (`SELECT`) operation, because applications read data far more often than they change it.
- Without indexes = **full table scan** (like searching a book with no table of contents).
# The Solution: Index 📇
- An **index** is like a book's **table of contents**.
	- A separate, special lookup table that the database creates for a *specific column* (like `name` in your `members` table example).
- It's *Pre-sorted* → direct access to rows without scanning full table
- This is why you add an index to columns that you search frequently (`필요한 column에 인덱스 사용`)
# The Trade-Off: Slower Writes
- "But if you add an index, everything except querying gets worse (trade-off)."
- While `SELECT` statements become much faster, `INSERT`, `UPDATE`, and `DELETE` operations become **slower**.
- **Why?** When you add a new member, the database doesn't just write one record to the `members` table. It now has to do **two** things:
    1. Add the row to the main table.
    2. Add a corresponding entry to the `name` index (and any other indexes on that table).
- Updating two data structures is always slower than updating one.
- 🧠 **Don’t over-index** → only index what's often queried