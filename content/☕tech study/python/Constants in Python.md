- use all caps to represent constants (there is no rlly any way, so it's more like convention)
```python 
PI = 3.14159
MAX_NUM = 100
```

- if you want actual enforcement (at least during development) you can use the `Final` type hint from the `typing` module
```python
from typing import Final

MAX_SPEED: Final[int] = 120
# This runs, but your editor will underline it in red
MAX_SPEED = 200
```
- the editor will scream error when u change the constant only when ur using the `typing` module

- If you are writing scripts or simple apps, just use `ALL_CAPS`. If you are working on a large, professional codebase, use `Final`