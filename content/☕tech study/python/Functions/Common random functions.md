- `random()` – Random float between 0.0 and 1.0.
	- `uniform(a, b)` – Random float between a and b (exclusive).
- `randint(a, b)` – Random integer between a and b (inclusive).
- `randrange(start, stop[, step])` – Random integer between start and stop - 1 with an optional step.
- Using random:
```python

from random import random

def return_random():
    num = random()
    print(f'num is {num}!')
    return 1 if num > 0.5 else 0

print(return_random())
# num is 0.8227931614156454!
# 1
```