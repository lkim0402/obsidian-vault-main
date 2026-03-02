- You are given an integer array `prices` where `prices[i]` is the price of NeetCoin on the `ith` day.
- You may choose a **single day** to buy one NeetCoin and choose a **different day in the future** to sell it.
- Return the maximum profit you can achieve. You may choose to **not make any transactions**, in which case the profit would be `0`.
# initial solution
```java
class Solution {
    public int maxProfit(int[] prices) {
        int minVal = Integer.MAX_VALUE;
        int minInx = 0;
        int res = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minVal) {
                minVal = prices[i];
                minInx = i;
            }
            if (prices[i] > minVal && i > minInx) {
                res = Math.max(prices[i] - minVal, res);
            }
        }
        return res;
    }
}
```
- critique
	- you don't need to check `i > minInx` because anything bigger than `minVal` is guaranteed to be at a later index.
		- so u dont actually need `minInx`!
	- you can make the 2nd if statement to be an else if
- this is actually a *greedy* algorithm
	- a greedy algorithm makes the locally optimal choice at each step with the hope of finding a global optimum
	- at each price point we make a decision

# second attempt
```java
class Solution {
    public int maxProfit(int[] prices) {
        int minVal = Integer.MAX_VALUE;
        int res = 0;
        for (int price : prices) {
            if (price < minVal) {
                minVal = price;
            } else if (price > minVal) {
                res = Math.max(price - minVal, res);
            }
        }
        return res;
    }
}
```
- it's still *greedy*
# official solution
```java
public class Solution {
    public int maxProfit(int[] prices) {
        int l = 0, r = 1;
        int maxP = 0;

        while (r < prices.length) {
            if (prices[l] < prices[r]) {
                int profit = prices[r] - prices[l];
                maxP = Math.max(maxP, profit);
            } else {
                l = r;
            }
            r++;
        }
        return maxP;
    }
}
```
- uses `l` and `r` pointers