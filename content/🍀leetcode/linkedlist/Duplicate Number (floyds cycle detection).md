You are given an array of integers `nums` containing `n + 1` integers. Each integer in `nums` is in the range `[1, n]` inclusive.
Every integer appears **exactly once**, except for one integer which appears **two or more times**. Return the integer that appears more than once.

# solution (floyds cycle detection)
## concept
- If you have an array, you can have each of the value of the array be a pointer that points to another index.
- Since each number is in the range `[1,n]` inclusive, we can be certain that the cycle will never be on the 0th index because no element can be 0 (and we're using the values as indices).
- Then, we can use slow/fast pointer to detect if there is a cycle.
- Example: `[1,2,3,1]`
	![|400](https://res.cloudinary.com/dwa6lkvcr/image/upload/v1769259490/image0_r2nmrl.jpg)
	  - The intersection of the slow/right pointers is 3. We leave the fast pointer  at that intersection.
- Now, can see that `p = x`.
		![](https://res.cloudinary.com/dwa6lkvcr/image/upload/v1769259490/image1_pnvuax.jpg)
	- NOTE: `N * c` where `N` is any integer >= 1.
		  - This is because for you to have a cycle, the  fast pointer MUST have done at least 1 extra lap to meet the 2nd pointer
- From here, we assign one pointer back to start, `slow2`. So we have 1 pointer at the start (`slow2`), and the slow pointer we left at the  intersection (`slow`).
- Then, we can keep  moving these 2 pointers by 1 until they intersect, and that will be the start  of the cycle.
## code
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = 0;
        int fast = 0;

        // finding intersection
        while (true) {
            slow = nums[slow];
            fast = nums[nums[fast]];
            if (slow == fast) {
                break;
            }
        }

        // reducing p and x to get start of cycle
        int slow2 = 0;
        while (true) {
            slow = nums[slow];
            slow2 = nums[slow2];
            if (slow == slow2) {
                return slow;
            }
        }
    }
}
```

# time complexity
- 1st phase (finding intersection)
	- slow pointer - slow pointer travels a distance strictly less than `N`, so the while loop runs fewer than `N` times
	- time complexity is `O(N)`
- 2nd phase (decreasing `p` and `x`, moving both slow pointers)
	- both pointers move 1 at a time, travel exactly distance `p`. 
	- `p < N`, time complexity is also `O(N)`
- Total: `O(N) + O(N) = O(N)`