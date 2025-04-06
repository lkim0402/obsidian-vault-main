https://docs.python.org/3/py-modindex.html
- `from` keyword: imports parts of a module
	- good rule of thumb: import only what you need!

```python
# Example: random module
import random
import random as rand # aliasing
from random import * # imports everything

rand.choice(['apple','banana','kiwi'])
rand.shuffle(['apple','banana','kiwi'])

from random import choice, shuffle # remove the word random!
choice(['apple','banana','kiwi'])
shuffle(['apple','banana','kiwi'])

from random import choice as c, shuffle as sh # you can do this too
```
The difference between `import random`  and `from random import *` 
-  `import random`: imports the entire `random` module
- `from random import *`: imports **all** the functions, classes, and variables from the `random` module directly into your current namespace
	- no need to prefix the functions with `random.`