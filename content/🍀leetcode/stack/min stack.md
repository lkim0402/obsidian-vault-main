```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.minStack = []
    """
    [3,0,1]
    [3,0,0]
    """

    def push(self, val: int) -> None:
        minVal = min(val, self.minStack[-1] if self.minStack else val)
        self.stack.append(val)
        self.minStack.append(minVal)

    def pop(self) -> None:
        self.stack.pop()
        self.minStack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.minStack[-1]
        
```

#### trick
- use a `minStack` that keeps track of the minimum value
- every time we push a value, we also keep track of the most recent smallest number
- getting the minimum at this point of `self.stack`