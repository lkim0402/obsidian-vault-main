```python
class GrumpyDict(dict):
	# no need for __init__ because it's inherited from the dictionary class

	# repr, missing, setitem are all in the official dict class
    def __repr__(self):
        print("NONE OF YOUR BUSINESS")
        #getting from official dict class
        return super().__repr__() 
	
    def __missing__(self, key):
        print(f"{key.upper()} AIN'T HERE!")
		
    def __setitem__(self, key, value):
        print(f"Fine....setting new values")
        # we use the __setitem__ from the dict class!!!!!!
        # this is just like "extending" the original base functionality
        super().__setitem__(key, value) 

test = GrumpyDict({"first":"Tom", "animal":"cat"})
# print(test)
print(test['city']) # "CITY AIN'T HERE!"
test['city'] = 'Seoul'
print(test)
```