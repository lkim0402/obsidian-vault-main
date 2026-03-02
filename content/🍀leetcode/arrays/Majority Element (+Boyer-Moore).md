Given an array `nums` of size `n`, return the **majority element**.

The majority element is the element that appears more than `⌊n / 2⌋` times in the array. You may assume that the majority element always exists in the array.

# my solution
```java
class Solution {
    public int majorityElement(int[] nums) {
        /*
        [5,5,1,1,1,5,5]
        map = {5:4, 1:3}
        
        */
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
        }

        int maxCount = 0;
        int res = 0;
        for (Map.Entry<Integer, Integer> entry: map.entrySet()) {
            Integer key = entry.getKey();
            Integer value = entry.getValue();
            if (value > maxCount) {
                maxCount = value;
                res = key;
            }
        }
        return res;
    }
}
```
- 2 pass solution
	- 1st, count the number's counts
		- ex) `{5:4, 1:3}` for `[5,5,1,1,1,5,5]`
	- 2nd, get the entrySet and compare `maxCount` and `res`
# optimized solution 1 (1 pass)
```java
class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int maxCount = 0;
        int res = 0;
        for (int n : nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
            if (map.get(n) > maxCount) {
                maxCount = map.get(n);
                res = n;
            }
        }
        return res;
    }
}
```
- one pass
	- you put/update the count to the map
	- and while you're updating it, you *also* check if the current count exceeds `maxCount`

# optimized solution 2 ( Boyer-Moore Voting Algorithm)
- how this works
	- maintain a `candidate` and a `count`
	- When we see the `candidate`, we increment the `count`; otherwise, we decrement it. When the `count` reaches `0`, we pick a new `candidate`. 
	- Since the majority element appears more than half the time, it will survive this elimination process and remain as the final candidate.
- algorithm
	- initialize `res` as the candidate and `count = 0`
	- for each element in `nums`
		- if `count == 0`, set `res = num`
		- if `num == res`, increment `count`; otherwise, decrement `count`
	- return `res` as the majority element

```java
class Solution {
    public int majorityElement(int[] nums) {
        int res = 0;
        int count = 0;
        for (int num : nums) {
            if (count == 0) {
                res = num;
            }
            count += (num == res) ? 1 : -1;
        }
        return res;
    }
}
```