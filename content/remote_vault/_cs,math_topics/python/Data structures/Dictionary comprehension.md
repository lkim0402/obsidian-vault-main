- [[Dictionary (Hashmap)]]
- Iterates over keys by default (we use `.items()` if we want to iterate over keys AND values)
```python 

{ (___ : ___) for ___ in ___ } # visualize with the ()

# Example 1
nums = {"first":1, "second":2, "third":3}
squared = {key:value **2 for key, value in nums.items()}
# {'first': 1, 'second': 4, 'third': 9}

# Example 2 (creating dictionary)
{num:num**2 for num in range(1,5)} # {1:1, 2:4, 3:9, 4:16}

# Example 3
str1 = "ABC"
str2 = "123"
combo = {str1[i]:str2[i] for index in range(0,len(str1))}
# {"A":1,"B":2,"C":3}
```

### With conditional logic
```python
num_list = [1,2,3,4]
{num: ("even" if num % 2 == 0 else "odd") for num in num_list}
# {1:"odd", 2:"even", 3:"odd", 4:"even"}
{num**2: ("even" if num % 2 == 0 else "odd") for num in num_list}
# {1: 'odd', 4: 'even', 9: 'odd', 16: 'even'}
```

### Examples
```python
""" Example 1 """
person = [["name", "Jared"], ["job", "Musician"], ["city", "Bern"]]
answer = {l[0]:l[1] for l in person} 
# {'name': 'Jared', 'job': 'Musician', 'city': 'Bern'}

""" Example 2 """
list1 = ["CA", "NJ", "RI"]
list2 = ["California", "New Jersey", "Rhode Island"]
answer = {list1[i]:list2[i] for i in range(0,len(list1))}
#{'CA': 'California', 'NJ': 'New Jersey', 'RI': 'Rhode Island'}

""" Example 3 """
# {'a': 0, 'e': 0, 'i': 0, 'o': 0, 'u': 0}
answer = {}.fromkeys("aeiou",0)
answer = {char:0 for char in "aeiou"}

""" Example 4 """
# { 65: 'A', 66: 'B',67: 'C',........ 90: 'Z'}
answer = {num:chr(num) for num in range(65,91)}
# chr() returns a string with the corresponding ASCII code

```