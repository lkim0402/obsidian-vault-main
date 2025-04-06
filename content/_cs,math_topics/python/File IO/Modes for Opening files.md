- We still have to `open` a file (open and read, or open and write)
- writing new stuff in file
	- for `w` and `a` flags, if no file is found it will **create a new file**
		- but for `r` (the default) flag if file doesn't exist it will raise an error
- `r` - Read a file (default)
- `w` - Write to a file (Overwrite)
- `a` - Appends to a file
- `r+` - Read AND write to a file (writing based on cursor)

## Writing+appending files
###  `w`
- `w` flag to `open()`
- OVERWRITES
```txt
(story.txt)
Hello!
```

```python
with open("story.txt", "w") as f:
    f.write("Hi.\n")
    f.write("Nice to meet you!\n") 
```

```
(story.txt)
Hi.
Nice to meet you!
```
-  It gets completely overwritten!

### `a`
- `a` flag to `open()`
- APPENDS
- You can't use `seek()` because `a` will always move the cursor to the end
```
(story.txt)
Original text!
```

```python
with open("story.txt", "a") as f:
	f.write("Hi.\n")
	f.write("Nice to meet you!\n")
```

```txt
(story.txt)
Original text!
Hi.
Nice to meet you!
```

### `r+`
- super common
- doesn't create a new file tho -> the idea is that it "updates" an existing file
```
(story.txt)
I WAS HERE FIRST!
```

```python
with open("story.txt", "r+") as f:
    f.write("Added using r+")
```

```
(story.txt)
Added using r+ST!
```
- The initial write (`f.write("Testing..\n")`) writes `"Testing..\n"` at the beginning of the file. 
- Then, `f.write("This is a test!")` writes over the beginning of the file again, overwriting the characters that were written by the first write.

### Exercises
````python
"""==========================================================="""
""" Copying content of file to file 2"""
def copy(file, file2):
    with open(file) as f:
        content = f.read()
    
    with open(file2, 'w') as f2:
        f2.write(content)

# other ppl's approaches (wtf)
def copy(file1, file2):
    with open(file1) as f1, open(file2, "w") as f2: 
	    f2.write(f1.read())

"""==========================================================="""
"""Copy and reverse content of file 1 to file2"""
def copy_and_reverse(file1, file2):
    with open(file1) as f1, open(file2, 'w') as f2:
        reverse = f1.read()[::-1]
        f2.write(reverse)

#other ppl's approaches (wtf22)
def copy_and_reverse(file1, file2):
    with open(file1) as f1, open(file2, "w") as f2:
		    f2.write(''.join(reversed(f1.read())))

"""==========================================================="""
"""returns dict with # of lines, words, and chars in the file"""
def statistics(file):
    ans = {}
    with open(file) as f:
        ans['lines'] = len(f.readlines())
        f.seek(0)
        ans['words'] = len(f.read().split())
        f.seek(0)
        ans['characters'] = len(f.read())
    
    return ans

# (wtf?)
def statistics(f):
    return {"lines": len(open(f).readlines()), "words":
	    len(open(f).read().split()), "characters": len(open(f).read())}
# official solution
    def statistics(file_name):
        with open(file_name) as file:
            lines = file.readlines()
     
        return { "lines": len(lines),
                 "words": sum(len(line.split(" ")) for line in lines),
                 "characters": sum(len(line) for line in lines) }
                 
"""==========================================================="""
"""Write a function called find_and_replace, which takes in a file name, a word to search for, and a replacement word"""
def find_and_replace(file, word1, word2):
    with open(file, 'r+') as f: # read AND write
        text = f.read()
        new_text = text.replace(word1, word2)
        
	    f.seek(0)
        f.write(new_text)
        f.truncate() 
        # removes/trim the leftovers
		# if len(word2) < len(word1) then the entire text gets shorter, so we
		# get rid of the leftovers
```