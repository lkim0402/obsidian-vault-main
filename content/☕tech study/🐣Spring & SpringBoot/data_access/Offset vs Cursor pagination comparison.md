# Pagination
- Resource
	- https://youtu.be/mvlzhBgGS4s?si=hMlpSSNhZgc8Iunk
- The ideal response
	- Nesting data in an attribute is useful
	- Allows us to add in other attributes useful for paging through the data, like `.next`
		- Examples of other attributes include
			- `count`: total number of elements
			- `next`:  next page URL
			- `prev`: prev page URL
```
{
	"data" : [...],
	"count": 721,
	"next": next pg url,
	"prev": prev pg url
}
```
# Types of Pagination
## 📌Offset based
- Use a `page` attribute or an `offset` + `limit` (they're the same thing behind the scenes, but it's just the client req)
### ***Approach 1: Using a `page` Attribute***
- In this method, the client simply requests a page number. The backend is responsible for calculating the correct `offset`.
1. Client Request: The client asks for a specific page (Ex: `GET /comments?page=3`)
2. Backend Logic
	- The backend has a fixed `page_size` (e.g., `page_size = 20`).
	- It calculates the `offset` with the formula: `offset = (page - 1) * page_size` 
		- `offset` = how many rows to skip, so if we put `OFFSET 40` this will start from 41st row
		- For our example: `offset = (3 - 1) * 20 = 40` -> this tells the database to skip the first 40 items
3. Database (SQL) Query: The backend query uses this calculated offset.
	- `SELECT * FROM comments ORDER BY id LIMIT 20 OFFSET 40`
	- This query retrieves items 41-60 (Page 1: 1-20, Page 2: 21-40, Page 3: 41-60).
4. Response
	- Next Page: The API response would point to `page=4`.
```json
{
  "next": "/comments?page=4",
  "previous": "/comments?page=2",
  "data": [...]
} 
```
- Custom Page Size: You can also let the client define the page size
	- Ex) `GET /comments?page=3&size=25`
	- The calculation for SQL query will change: `offset = (3 - 1) * 25 = 50`.
### ***Approach 2: `offset` + `limit`***
- The client is given more control and specifies the `limit` and `offset` directly
	- more flexible but requires the client to manage more state
 1. Client request: `GET /comments?limit=30&offset=120`
2. Backend Logic
    - (Very simple) The backend just (safely) passes these values directly to the database.
    - The server should still enforce a **maximum limit** (e.g., `max_limit = 100`) to prevent abuse.
3. Database (SQL) Query
	- `SELECT * FROM comments ORDER BY id LIMIT 30 OFFSET 120;`
4. Response
    - The "next" link is calculated by adding the `limit` to the `offset`.
    - `next_offset = current_offset + current_limit` (e.g., 120 + 30 = 150)
```json
{
  "next": "/comments?limit=30&offset=150",
  "previous": "/comments?limit=30&offset=90",
  "data": [...]
}
```
### Limitations
- Simple but problematic
- **Inconsistent Data (The "Page Shift" Problem)**
    - `OFFSET` is just a **row counter** -> has no concept of _what_ the data is, it just skips a number of rows
    - **If data changes** (new rows added or old rows deleted) while the user is paging, the dataset shifts underneath them.
    - Result => The user can experience **missing data** (items get skipped) or **repeating data** (items appear on two different pages).
	- Example
		- You are on Page 1 (items 1-20). You click "Next." 
		- Before your request for Page 2 (`OFFSET 20`) arrives, 5 new items are added to the _beginning_ of the list.
		- Your `OFFSET 20` query now skips the 5 new items _plus_ the first 15 items you saw on Page 1. The last 5 items from Page 1 (items 16-20) will now appear at the _top_ of your Page 2. **You see repeated data.**
- **Poor Database Performance**
    - `OFFSET` is very slow on large tables, especially with high page numbers (e.g., `OFFSET 1000000`).
    - To execute `OFFSET 1,000,000`, the database still has to **read all 1,000,000 rows** from disk and count them, just to throw them away.
    - This performance degrades linearly as the page number increases, making deep pagination unusable.
## 📌 Cursor based
- Cursor-based pagination (also known as **keyset pagination**) is a method that avoids the problems of `OFFSET` by using a "cursor"—a stable, unique pointer to the last item the user saw.
	- Using a cursor (pointer) to the "last record seen"
	- Identifying a specific row and going from there
	- So we're immune to data being removed/added
### ***How it works***
1. **Initial Request** 
	- The client makes a normal request for the first page.
    - `GET /comments?limit=20`
    - SQL: `SELECT * FROM comments ORDER BY id LIMIT 20;`
2. **Server Response** 
	- The server returns the first 20 items and, most importantly, a "cursor" pointing to the _last item_ in that list (e.g., the 20th item has `id=12345`).
3. **Next Request:** The client uses this cursor to ask for the next page.
    - `GET /comments?limit=20&after=12345`
4. **Backend Logic & SQL:** The backend uses this `after` value in a `WHERE` clause.
    - SQL: `SELECT * FROM comments WHERE id > 12345 ORDER BY id LIMIT 20;`
	- This query is extremely fast because the database can use an index to instantly "seek" to `id = 12345` and start reading from there. It doesn't need to read and discard 1,000,000 rows like `OFFSET` would.
### Implementation: Cursor vs tokens
- How you pass the cursor is an implementation detail
1. *Simple (but bad) Approach: Exposing IDs*
	- This is the simple example from above.
	- **Request:** `GET /comments?after-id=12345`
	- **Response:** `next: "/comments?after-id=12378"`
	- **Downsides**
	    - **Bad Practice:** Exposes internal database IDs to the client.
	    - **Confusing:** Hard to manage direction (e.g., `after` vs. `before`).
	    - **Limited:** Only works if you're sorting by one column (`id`). What if you sort by `createdAt` and then `id`?
2.   *Best Practice: Continuation Tokens*
	- The professional way to implement it -> A "continuation token" is an opaque (unreadable) string that encodes all the information needed to get the next page.
	- **Token:** `continuation = abc123xyz`
		- This `abc123xyz` string is actually a **Base64-encoded** version of the last item's data, like `{"createdAt": "2025-11-17T13:30:00", "id": 12378}`.
	- **Request:** `GET /comments?limit=20&continuation=abc123xyz`
	- **Backend Logic**
	    1. Backend decodes `abc123xyz` back into `{"createdAt": "2025-11-17T13:30:00", "id": 12378}`.
	    2. It uses this info to build a complex, stable query.
		    - `SELECT * FROM comments WHERE (createdAt, id) > ('2025-11-17T13:30:00', 12378) ORDER BY createdAt, id LIMIT 20;`
	- **Response:**
	    - The `next` attribute is critical. The client doesn't need to understand the token; it just needs to save the `next_continuation_token` from the response and send it back for the next page.
### Limitations
- Limited to certain columns
	- it has to be `UNIQUE`, `NOT NULL`, unchanging, and indexed
	- commonly used: `Id` or a `createdAt` timestamp
- No arbitrary page jump
	- it's pretty just much `next` and `previous`, like a **linkedlist**!
	- u can't just jump like 7 pages forward
- when to use
	- infinite scroll with load more
	- it's also the ideal choice for large datasets