
> [!Overview]
> Defines/modifies **DB schema** (structure) in [[SQL]]
> - Create/drop tables, alter columns => basically CRUD the tables
> - Changes are saved immediately.


- Commands:  `CREATE`, `ALTER`, `TRUNCATE`, `DROP`
```sql
-- Create the initial 'products' table
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2)
);

-- Add a new column for stock quantity
ALTER TABLE products ADD COLUMN stock_quantity INT;

--  Delete the data in the table, but keep the table (initialization)
TRUNCATE TABLE products;

-- Delete the 'products' table completely
DROP TABLE products;
```

# `CREATE`

|**Keyword**|**Description**|
|---|---|
|`SERIAL`|Auto-increment integer (PostgreSQL only)|
|`VARCHAR(n)`|Stores a string up to _n_ characters|
|`NOT NULL`|Value must not be empty (required)|
|`UNIQUE`|Ensures values are not duplicated|
|`TIMESTAMP`|Represents date and time|
|`DEFAULT`|Sets a default value if none is provided|