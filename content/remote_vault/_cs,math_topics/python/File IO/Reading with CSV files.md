> CSV - Comma separated values
> - 1st row in any csv file contains the header (what each column is)
> - We use Python's **built-in CSV module** that helps us read/write CSVs more easily

### CSV modules
- `reader`  
	- lets you iterate over rows of the CSV as lists
	- gives each row as lists
- `DictReader`
	- lets you iterate over rows of the CSV as OrderedDicts
	- each row is a an OrderedDict (dictionary with order)
- Both of them can accept csv files that aren't separated by a comma (like a pipe |)
	- Then just `csv_reader = reader(file, delimmiter ="|")`

### `reader`

```csv
Name,Country,Height (in cm)
Ryu,Japan,175
Ken,USA,175
Chun-Li,China,165
Guile,USA,182
E. Honda,Japan,185
Dhalsim,India,176
Blanka,Brazil,192
Zangief,Russia,214
```

```python
from csv import reader

with open('fighters.csv') as file:
	csv_reader = reader(file) 
	next(csv_reader) #moves forward once to skip header
	for row in csv_reader:
		# each row is a list
		print(row)

# ['Name', 'Country', 'Height (in cm)']
# ['Ryu', 'Japan', '175']
# ...
# ['Zangief', 'Russia', '214']
```
- `csv_reader` is an iterator!
	- If you finish it and exhaust it, it won't output anything
	- ` print(csv_reader)`  will give you `<_csv.reader object at 0x7ff2f9149460>`
- If you want to make the data into a list, then `data = list(csv_reader)`

### `DictReader`
```python
from csv import DictReader

with open('fighters/.csv') as file:
	csv_reader = DictReader(file) 
	next(csv_reader) #moves forward once to skip header
	for row in csv_reader:
		print(row)

# {'Name': 'Ken', 'Country': 'USA', 'Height (in cm)': '175'}
# {'Name': 'Chun-Li', 'Country': 'China', 'Height (in cm)': '165'}
# ...
# {'Name': 'Zangief', 'Country': 'Russia', 'Height (in cm)': '214'}
```
- OrderedDict
	- A special collection in Python
	- dictionary that is ordered
	- but we don't have to use this because after python 3.7 dictionaries are all ordered!

### Example
changing one dataset column - cm to inches
```python
from csv import DictReader, DictWriter

def cm_to_in(cm):
	return float(cm) * 0.393701
	
with open("file1.csv") as f:
	csv_reader = DictReader(file)
	fighters = list(csv_reader) # to use for later
	# file1.csv has headers Name, Country, Height(cm)

with open('file2.csv') as f:
	headers = ["Name", "Country", "Height(in)"] # new header we will use
	csv_writer = DictWriter(file, fieldnames = headers)
	csv_writer.writeheader()
	for fighter in fighters: #looping through the list made earlier
		csv_writer.writerow({
			"Name": fighter["Name"],
			"Country": fighter["Country"],
			"Height(in)": cm_to_in(fighter["Height(cm)"])
		})
```

- `DictReader` automatically uses the 1st row of the CSV file as the header, so if you want to loop through your data just loop through from the start 