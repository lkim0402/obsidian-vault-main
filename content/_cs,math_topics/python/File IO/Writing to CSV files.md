- Write using lists or dictionaries

## `writer`
- creates a writer object for writing to the CSV
- `writerow` - method on a writer to write a row to the CSV
```csv
Name,Country,Height (in cm)
Ryu,Japan,175
Ken,USA,175
Chun-Li,China,165
Guile,USA,182
```

```python
from csv import writer

with open('fighters.csv', "w") as file:
	csv_reader = writer(file) 
	csv_reader.writerow(["Test1","Test2"])
```

```csv
Test1,Test2
```
- `writerow` completely overwrites because the flag of `open()` is `"w"`
- If the flag is `"a"`
```csv
Name,Country,Height (in cm)
Ryu,Japan,175
Ken,USA,175
Chun-Li,China,165
Guile,USA,182
Test1,Test2
```
#### Another example
```python
"""
we're getting all data from file1, and transfer the data but UPPERCASED
"""
import csv reader, writer

with open('file1.csv') as file:
	csv_reader = reader(file)
	data = [[data.upper() for data in row] for row in csv_reader]

with open('file2.csv', "w") as file:
	csv_writer = writer(file)
	for uppercase_data in data:
		csv_writer.writerow(uppercase_data)

```
## `DictWriter`
- `DictWriter` - creates a writer object for writing using dictionaries
- `fieldnames` - kwarg for the DictWriter specifying headers
- `writeheader` - method on a writer to write header row
- `writerow` - method on a writer to write a row based on a dictionary

```python
from csv import DictWriter

with open('example.csv', 'w') as file:
	headers = ["Character", "Age"]
	csv_writer = DictWriter(file, fieldnames = headers)
	csv_writer.writeheader()
	csv_writer.writerow({
		"Character": "Kirby",
		"Age": 100
	})
	csv_writer.writerow({
		"Character": "Yoshi",
		"Age": 100
	})

	# if you want to print out the new file data:
	csv_reader = DictReader(file)
	file.seek(0)
	for row in csv_reader:
		print(row)
```

- This then creates a new file `example.csv` because of the `w` flag (and `example.csv` doesn't exist:
```
Character,Age
Kirby,100
Yoshi,100
```


### Examples
For this exercise, you'll be working with a file called `users.csv` . Each row of data consists of two columns: a user's first name, and a user's last name.

Implement the following function:

`update_users` : Takes in an old first name, an old last name, a new first name, and a new last name. Updates the `users.csv` file so that any user whose first and last names match the old first and last names are updated to the new first and last names. The function should return a count of how many users were updated.

```python
'''
update_users("Grace", "Hopper", "Hello", "World") # Users updated: 1.
update_users("Colt", "Steele", "Boba", "Fett") # Users updated: 2.
update_users("Not", "Here", "Still not", "Here") # Users updated: 0.
'''

from csv import reader, writer

def update_users(old_first, old_last, new_first, new_last):
    with open('users.csv') as f:
        csv_reader = reader(f)
        data = list(csv_reader)
        
    count = 0
    with open('users.csv', 'w') as f:
        csv_writer = writer(f)
        for user in data:
            if (user[0] == old_first) and (user[1] == old_last):
                csv_writer.writerow([new_first,new_last])
                count += 1
            else:
                csv_writer.writerow(user)
                
    return f"Users updated: {count}."
```

`delete_users` : Takes in a first name and a last name. Updates the `users.csv` file so that any user whose first and last names match the inputs are removed. The function should return a count of how many users were removed.

```python
'''
delete_users("Grace", "Hopper") # Users deleted: 1.
delete_users("Colt", "Steele") # Users deleted: 2.
delete_users("Not", "Here") # Users deleted: 0.
'''

from csv import reader, writer

def delete_users(first, last):
    with open('users.csv') as f:
        csv_reader = reader(f)
        data = list(csv_reader)
        
    count = 0
    with open('users.csv', 'w') as f:
        csv_writer = writer(f)
        for user in data:
            if (user[0] == first) and (user[1] == last):
                count += 1
            else:
                csv_writer.writerow(user)
                
    return f"Users deleted: {count}."
```