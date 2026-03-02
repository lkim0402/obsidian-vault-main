
## correct
```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        """
        ["1","2","+","3","*","4","-"]
        """
        numStack = []
        for t in tokens:
            if t in "+-*/":
                num1 = numStack.pop()
                num2 = numStack.pop()
                if t == "+":
                    val= num1 + num2
                elif t == "-":
                    val = num2 - num1
                elif t == "/":
                    val = int(float(num2) / num1)
                elif t =="*":
                    val = num1 * num2
                numStack.append(val) 
            else:
                numStack.append(int(t))
        
        return numStack[0]
```
- reverse the if else case (from the wrong answer below)
- be careful with the order of operations
	- left - right
	- left / right
- Assume that division between integers always truncates toward zero.
	- `int(float(num2) / num1)`
	- `int(num2 / num1)`
- O(n)
- one note:
	- it's good to debug where it might have gone wrong, because i didn't really know that my error was from `t.isnumeric()`
## wrong!!
```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        """
        ["1","2","+","3","*","4","-"]
        """
        numStack = []
        for t in tokens:
            if t.isnumeric():
                numStack.append(int(t))
            else: # is an operand
                val = 0
                if (numStack):
                    num1 = numStack.pop()
                    num2 = numStack.pop()
                    if t == "+":
                        val = num1 + num2
                    elif t == "-":
                        val = num2 - num1
                    elif t == "/":
                        val = int(float(num2) / num1)
                    elif t =="*":
                        val = num1 * num2
                numStack.append(val)
        
        return numStack[0]
```
- my initial answer, but this doesnt work because `t.isnumeric()` doesnt evaluate correctly for negative numbers like `-4`