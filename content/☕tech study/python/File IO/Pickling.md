> The `pickle` module in Python is used for serializing and deserializing Python objects.

> A **byte stream** is a sequence of bytes (numbers between 0 and 255) that represents raw binary data.
> - represents the object's structure in memory
> - A **text stream** represents characters (using encodings like UTF-8) 

- **Serialization** (or "pickling") means converting a Python object into a byte stream (a format that can be stored in a file, transmitted over a network, etc.).
- **Deserialization** (or "unpickling") is the reverse process of converting the byte stream back into a Python object.
- Pickling allows you to save complex data structures, such as lists, dictionaries, or even custom Python class instances, to a file or transfer them between different programs.

```python
""" ===== Pickling (Serialization) ===== """
import pickle

# Example dictionary
data = {'Name': 'Alice', 'Age': 25, 'Country': 'USA'}

# Open a file to save the data
with open('data.pkl', 'wb') as file:
    # Serializes the data and saves it to file
    pickle.dump(data, file)  


"""Unpicklng (Deserialization) """
# Open the file and load the data back
with open('data.pkl', 'rb') as file:
    # Deserializes the byte stream back into a Python object
    loaded_data = pickle.load(file)  

print(loaded_data)
# {'Name': 'Alice', 'Age': 25, 'Country': 'USA'}
```
- In `open()`, we use `wb` because we're writing it as binary and `rb` because we're reading as binary
- file extensions don't matter - you can use `.pkl` or `.pickle`
- `dump` -> serialization
- `load` -> deserialization

### Using Pickling with JSON
- Python has a module named `json` so we can work with jsons
	- We can encode and decode from python to json and vise versa
```python
import json
```


### Common Use Cases
- Saving model states (e.g., machine learning models) to disk for later use.
- Storing large Python data structures for persistence.
- Transferring Python objects between different programs or systems.
### Limitations
- The `pickle` module is Python-specific, meaning the pickled data can only be understood by Python programs.
- Pickling can be insecure if you unpickle data from untrusted sources, as it may execute arbitrary code.