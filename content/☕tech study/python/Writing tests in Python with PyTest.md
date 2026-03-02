
# PyTest
>[!Pytest]
>The standard, and the most popular framework for writing tests in Python

- simply type `pytest` in the terminal in ur application folder, and it will run all the unit tests it can find in the directory
	- It finds the test using *naming conventions* 
	- add `test_` to the front to both the file and the class!

```bash
pytest
pytest test_shopping_cart.py # all tests here
pytest test_shopping_cart.py::test_can_add_item_to_cart # specific test
pytest -s # prints print statements
```

- Each unit test is a function
```python
from shopping_cart import ShoppingCart

def test_can_add_item_to_cart():
    cart = ShoppingCart()
    cart.add("apple")
    assert cart.size() == 1
```
## Good practices
```python
# 1
def test_when_add_more_than_max_items_should_fail():
    cart = ShoppingCart(5) # max_size = 5
    with pytest.raises(OverflowError):
        for _ in range(6):
            cart.add("apple")

# 2
def test_when_add_more_than_max_items_should_fail():
    cart = ShoppingCart(5) 
    for _ in range(5):
        cart.add("apple")
    
    with pytest.raises(OverflowError):
        cart.add("apple")
```
- Let's say that you cannot add more things than the `max_size`, passed in the constructor
- `with pytest.raises(OverflowError):`
	- Allows you to throw error
	- 1st - we don't know when the error is raised
	- 2nd - we know exactly when the error is raised 
## Fixtures
- Provides a defined, reliable and consistent context for the tests
	- Could include environment (for example a database configured with known parameters) or content (such as a dataset)
- Use `@pytest.fixture`
	- Add to the argument to the test functions
	- It is created newly for *eact test function*
```python
@pytest.fixture
def cart():
    return ShoppingCart(5)

def test_can_add_item_to_cart(cart):
    cart.add("apple")
    assert cart.size() == 1
```
### Scopes
- **Function** (Set up and tear down once for each test function)
	- Default
- **Class** (Set up and tear down once for each test class)
- **Module** (Set up and tear down once for each test module/file)
- **Session** (Set up and tear down once for each test session i.e comprising one or more test files)

```python
@pytest.fixture(scope='module')
```
## Mocking Dependencies (`unittest.mock`)
- Fake the behavior

```python
class ItemDatabase:
    def __init__(self) -> None:
        pass

    def get(self, item:str) -> float:
        pass
```

```python
def test_can_get_total_price(cart):
    cart.add("apple")
    cart.add("orange")

    item_database = ItemDatabase()
    item_database.get = Mock(return_value = 3.0)

    price_map = {
        "apple": 1.0,
        "orange": 2.0
    }
    assert cart.get_total_price(price_map) == 3.0
```
- Scenarios for `ItemDatabase`
	-  It's created by someone else in your team so you don't have the code yet but you have to make the test cases
	- Its connection is unstable

Using the `side_effect` argument
- A function that takes whatever the mock receives
- we can do all sorts of things
```python
def test_can_get_total_price(cart):
    cart.add("apple")
    cart.add("orange")

    item_database = ItemDatabase()

    def mock_get_item(item: str):
        if item == "apple":
            return 1.0
        elif item == "orange":
            return 2.0

    item_database.get = Mock(side_effect=mock_get_item) # put it in here!
    assert cart.get_total_price(item_database) == 3.0
```

```python
from typing import List

class ShoppingCart:
    def __init__(self, max_size: int) -> None:
        self.items = []
        self.max_size = max_size

	...

    def get_total_price(self, price_map):
        totalPrice = 0
        for item in self.items:
            totalPrice += price_map.get(item)
        return totalPrice
```

# Advanced Mocking: `mocker` and `patch`
- Sometimes you need to mock something _before_ an object is even created, or mock a function that's imported directly. The standard way to do this is with `patch`, which is provided by the `pytest-mock` plugin via the `mocker` fixture.
	- **`mocker`**: A PyTest fixture that provides a simple wrapper around `unittest.mock`.
	- **`mocker.patch()`**: The "stunt double director." It swaps a _real_ object/function/class with a `MagicMock` for the duration of your test.
	- **`MagicMock`**: A "better" `Mock`. It automatically fakes "magic methods" (like `__len__`, `__str__`, etc.), which is very useful. `mocker.patch()` uses this by default.
- The Most Important Rule of Patching
	- You must **patch where the object is _used_ (imported), not where it is _defined_."**
## Example
Let's assume  `ShoppingCart` is  a "hard" dependency and _imports_ the `ItemDatabase` directly.

```Python
# shopping_cart.py
from item_database import ItemDatabase # <-- Hard dependency!

class ShoppingCart:
    # ... (init, add, size...)
    def get_total_price(self):
        db = ItemDatabase() # <-- Creates the dependency internally
        total_price = 0
        for item in self.items:
            total_price += db.get(item) # Calls the real (or mocked) db
        return total_price
```

You can't pass a mock in. You _must_ patch `ItemDatabase` _where `shopping_cart.py` uses it_.
```Python
# test_shopping_cart.py

# Your fixture from before
@pytest.fixture
def cart():
    c = ShoppingCart(5)
    c.add("apple")
    c.add("orange")
    return c

# We add the 'mocker' fixture as an argument
def test_get_total_price_with_mocker(cart, mocker):
    
    # 1. Define the behavior of our mock database instance
    mock_db_instance = mocker.MagicMock()
    
    def mock_get_item(item: str):
        if item == "apple": return 1.0
        if item == "orange": return 2.0
    
    # Use side_effect just like in your notes
    mock_db_instance.get.side_effect = mock_get_item

    # 2. This is the key:
    # We patch 'ItemDatabase' *inside* the 'shopping_cart' module.
    # We tell it: "When ShoppingCart calls 'ItemDatabase()',
    # don't create a real one. Instead, return our mock_db_instance."
    mocker.patch(
        'shopping_cart.ItemDatabase', # <-- Path to where it's USED
        return_value=mock_db_instance
    )

    # 3. Now when we call this, it will use the mock
    assert cart.get_total_price() == 3.0

    # 4. We can also make assertions on the mock instance
    mock_db_instance.get.assert_any_call("apple")
    mock_db_instance.get.assert_any_call("orange")
```
# `monkeypatch` vs. `mocker`
- Both are *fixtures*
	- **`mocker`** (`pytest-mock`)
		- Use this to **mock objects and functions**. 
		- Its main purpose is to "spy" on your code's dependencies. You check _if_ they were called and _what_ they were called with (e.g., `assert_called_once_with(...)`).
	- **`monkeypatch`** (`pytest` built-in)
		- Use this to **change the _environment_ of your test**. You don't "spy" on it; you just set its value.
		- Use it for:
			- Setting environment variables (`monkeypatch.setenv`)
			- Changing `sys.argv` to simulate command-line arguments (`monkeypatch.setattr`)
			- Changing global settings or constants (`monkeypatch.setattr`)
## `monkeypatch` Example
```Python
# run_bench.py
import sys
import argparse

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--model_path', type=str, required=True)
    parser.add_argument('--repeat', type=int, default=1)
    args = parser.parse_args() # This reads from sys.argv

    # ... (functions we want to mock) ...
    compile_executorch_model_for_path(args.model_path)
    executorch_get_benchmark(args.model_path, args.repeat)

# ... (other functions) ...
```

- The Test File
	- Here, you use **both** fixtures
		- `mocker` to spy on the functions.
		- `monkeypatch` to set the environment (`sys.argv`).
```Python
# test_run_bench.py
import sys
import run_bench # Import the module to be tested

def test_main_function(mocker, monkeypatch):
    # 1. Use MOCKER to spy on the functions
    mock_compile = mocker.patch('run_bench.compile_executorch_model_for_path')
    mock_benchmark = mocker.patch('run_bench.executorch_get_benchmark')
    
    # 2. Use MONKEYPATCH to set the environment
    # We are simulating a user running:
    # python run_bench.py --model_path mobilenetv2.pte --repeat 10
    test_args = ['run_bench.py', '--model_path', 'mobilenetv2.pte', '--repeat', '10']
    monkeypatch.setattr(sys, 'argv', test_args)
    
    # 3. Run the function
    run_bench.main()
    
    # 4. Assert the MOCKS were called correctly
    mock_compile.assert_called_once_with('mobilenetv2.pte')
    mock_benchmark.assert_called_once_with('mobilenetv2.pte', 10)
```