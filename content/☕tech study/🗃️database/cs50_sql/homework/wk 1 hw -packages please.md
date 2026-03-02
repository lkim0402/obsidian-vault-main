- This is a document of trying to improve my SQL queries, because I've hard-coded them (which is bad)
- https://cs50.harvard.edu/sql/psets/1/packages/

# the lost letter
original
- need to find the address and type of address
```sqlite
-- find the address where it's sent
SELECT "id" FROM "packages" WHERE "from_address_id" = (
    -- find the package (to address = 854, package id = 384)
    SELECT "id" FROM "addresses"
    WHERE "address" = "900 Somerville Avenue"
)
AND "contents" = "Congratulatory letter";
-- find where the package was dropped (854)
SELECT "package_id","address_id","action" FROM "scans" WHERE "package_id" = 384;
```

improved (using `JOIN`)
```sqlite
SELECT a.address, a.type
FROM packages p
JOIN scans s ON s.package_id = p.id
JOIN addresses a ON a.id = s.address_id
WHERE p.contents = "Congratulatory letter"
AND a.address = "900 Somervile Avenue";
```

# the devious delivery
original
- need to find the address & content of delivery
```sqlite
-- find content, id of the package
SELECT "id","contents" FROM "packages" WHERE "id" = (
    -- find package id
    SELECT "id" FROM "packages"
    WHERE "from_address_id" IS NULL; --5098
);
SELECT "address_id" FROM "scans" WHERE "action" = "Drop" AND "package_id" = 5098; --348
SELECT "type" FROM "addresses" WHERE "id" = 348;
```

improved (using `JOIN`)
```sqlite
SELECT p.contents, a.type 
FROM packages p
JOIN scans s ON s.package_id = p.id
JOIN addresses a ON a.id = s.address_id
WHERE s.action = 'Drop' 
AND p.from_address_id IS NULL;
```
# forgotten gift
- Oh, excuse me, Clerk. I had sent a mystery gift, you see, to my wonderful granddaughter, off at 728 Maple Place. That was about two weeks ago. Now the delivery date has passed by seven whole days and I hear she still waits, her hands empty and heart filled with anticipation. I’m a bit worried wondering where my package has gone. I cannot for the life of me remember what’s inside, but I do know it’s filled to the brim with my love for her. Can we possibly track it down so it can fill her day with joy? I did send it from my home at 109 Tileston Street.

original
- need to find the content and the driver
```sqlite
-- to and from addresses (9873 -> 4983)
SELECT * FROM "addresses" WHERE "address" = "728 Maple Place" OR "address" = "109 Tileston Street";
-- get package id
SELECT * FROM "packages" WHERE "from_address_id" = 9873;
-- get driver id who picked up the most recent
SELECT * FROM "scans" WHERE "package_id" = 9523;
-- get name of driver
SELECT "name" FROM "drivers" WHERE "id" = 17;
```

trial (using `JOIN`) - wrong 
```sqlite
SELECT p.contents, d.name
FROM packages p
JOIN scans s ON p.id = s.package_id
JOIN addresses a ON a.id = s.address_id
JOIN drivers d ON d.id = s.driver_id
WHERE a.address = "109 Tileston Street"
ORDER BY s.timestamp DESC;
```
- `109 Tileson Street` is the sender's home address. 
	- we're asking `addresses`, which was joined on `scans`, to filter by that address
	- but there might have been many other scans that's *not the forgotten gift* dropped/picked up in that address

success 
```sqlite
--SELECT p.contents, d.name
SELECT *
FROM packages p
-- group by packages only sent
JOIN addresses a ON a.id = p.from_address_id 
JOIN scans s ON p.id = s.package_id
LIMIT 4;

```