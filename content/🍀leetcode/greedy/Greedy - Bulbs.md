https://www.interviewbit.com/problems/interview-questions/

**N** light bulbs are connected by a wire.  
Each bulb has a switch associated with it, however due to faulty wiring, a switch also changes the state of all the bulbs to the right of current bulb.  
Given an initial state of all bulbs, find the minimum number of switches you have to press to turn on all the bulbs.  
You can press the same switch multiple times.  
  
**Note** : `0` represents the bulb is off and `1` represents the bulb is on.


```python
class Solution:
    # @param A : list of integers
    # @return an integer
    def bulbs(self, A):
        cost = 0
        
        for b in A:
	        # this part
            if cost % 2 == 0: 
                b = b
            else:
                b = not b
            
            if b == 0:
                cost += 1
        
        return cost
```
- 핵심
	- cost = # of flips
	- the 1st if else case "sets" the bulbs according to how many times it flipped
- This is a greedy algorithm because
	- we are making the locally optimal choice at each step
		- we **make a choice based only on current information**
		- **never** reconsider that choice
	- **If a bulb at position `i` is off, we MUST flip it**
		- we can't go back, unlike dynamic programming or backtracking
- The greedy choice happens to be optimal because of the problem structure:
	- We can only flip rightward
	- Each position has only one chance to be fixed
	- So the "greedy" choice (fix immediately) is actually the only sensible choice

