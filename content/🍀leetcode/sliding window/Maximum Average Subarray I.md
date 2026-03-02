- You are given an integer array `nums` consisting of `n` elements, and an integer `k`.
- Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return _this value_. Any answer with a calculation error less than `10-5` will be accepted.
- Constraints:
	- `n == nums.length`
	- `1 <= k <= n <=`${10}^5$
	- ${-10}^4$ `<= nums[i]` <= ${10}^4$
# my solution
```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        double maxAvg = Integer.MIN_VALUE;
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (i >= k - 1) {
                double avg = (double) sum / k;
                maxAvg = Math.max(maxAvg, avg);
                sum -= nums[i - k + 1]; 
            }
        }
        return maxAvg;
    }
}
```
- even though it's optimal in terms of time complexity `O(N)`, we can make it better
	- avoid division in the loop
		- division is computationally more expensive than `+` or `-`
		- since `k` is constant, the highest sum automatically means highest avg value
		- so find `maxSum`, then return `maxSum/k` at the end
	- cleaner structure (the *standard structure*)
		- instead of putting `if (i >= k - 1)` in the loop which runs for every single element, we can:
			- calculate the sum of the first `k` elements separately
			- start the loop from the `k`-th element
	- initialization 
		- initialization of `maxSum` with the first window's sum is safer
# better solution
```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int sum = 0;
        // using 1st window
        for (int i = 0; i < k; i++) {
            sum += nums[i];
        }

        // iterating with remaining windows
        double maxSum = sum;
        for (int i = k; i < nums.length; i++) {
            sum += nums[i];
            sum -= nums[i - k];
            maxSum = Math.max(maxSum, sum);
        }
        return maxSum / k;
    }
}
```

- one tiny safety tip
	- in this specific problem, the constraints say `num[i]` is up to ${10}^4$ and `k` is up to ${10}^5$. 
	- Scenario
		- imagine a window where every single number is the maximum value ${10}^4$.
		- To find the `sum` of that window, you multiply the number of items by the value (${10}^4$ per item)
			- $100,000 \times 10,000 = 1,000,000,000$ (1 billion)
	- The java `int` limit
		- it has a max limit of approximately 2 billion
		- so 1 billion < 2 billion -> rn it's safe to use `int` for `sum`, but if the constraints used a bit more higher number, we can't use `int` since it would give *integer overflow*. 
			- we would have to use `long` for `sum` instead
	- *so safe habit for sliding window sums* -> `long sum`

# time complexity
- `O(N)`
	- You iterate through the array exactly once (the first loop runs `k` times, and the 2nd loop runs `N - k` times. `k + N - k = N`