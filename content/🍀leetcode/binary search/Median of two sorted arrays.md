- [[🍀Leetcode]]
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] A = nums1;
        int[] B = nums2;
        int total = A.length + B.length;
        // int half = total / 2;
        int half = (total + 1) / 2; // we + 1 to make sure the median is always on the left partition

        // make A the shorter one
        if (B.length < A.length) {
            int[] tmp = A;
            A = B;
            B = tmp;
        }

        int l = 0;
        int r = A.length;
        // it's not A.length - 1 because i or j represents the start of the right (the "cut")
        // we need this to consider the case where ALL elements are in the left side (and 0 to the right)
        while (l <= r) {
            int i = (l + r) / 2; // start of ARight.. (or # of elems to put in Aleft)
            int j = half - i; // start of BRight (or # of elems to put in Bleft)

            int Aleft = i > 0 ? A[i - 1] : Integer.MIN_VALUE;
            int Aright = i < A.length ? A[i] : Integer.MAX_VALUE;
            int Bleft = j > 0 ? B[j - 1] : Integer.MIN_VALUE;
            int Bright = j < B.length ? B[j] : Integer.MAX_VALUE;

            if (Aleft <= Bright && Bleft <= Aright) {
                if (total % 2 != 0) { // total is odd, return the max of the lefts
                    return Math.max(Aleft, Bleft);
                } else { // total is even, we get the middle (max of lefts + min of rights) / 2
                    return (Math.max(Aleft, Bleft) + Math.min(Aright, Bright)) / 2.0;
                }
            } else if (Aleft > Bright) {
                r = i - 1;
            } else {
                l = i + 1;
            }
        }
        return -1;
    }
}
```

- Concept
	- Imagine we merged those 2 arrays. We want to get the Left partition and the Right partition, then get the median.
	- But since we can’t merge, we need to find the partition (cut) that divides the combined set of numbers into 2 equal halves (left and right)
- Strategy
		![|500](https://res.cloudinary.com/dwa6lkvcr/image/upload/v1768919079/IMG_0602_ovssrp.png)
	- The main idea is that we have a Left and Right partition, and we check `Aleft <= Bright && Bleft <= Aright`
		- If so, that means we have successfully found the left and right partition
			- If total is an odd number, then get  `Main.min(Aright, Bright)`
			- If total is an even number, get `(Math.max(Aleft, Bleft) + Math.min(Aright, Bright)) / 2`
		- If not, if `Aleft > Bright`
			- That means when we combine all our lefts, in the left partition we have an element that is bigger than one element on the right partition
			- we have to shrink `Aleft` and expanding `Bleft`, so `r = i - 1`
		- else (`Bleft > Aright`)
			- That means when we combine all our lefts, in the left partition we have an element that is bigger than one element on the right partition
			- we have to shrink `Bleft` and expanding `Aleft` -> `l = i + 1`
- Things to note
	- `int total = A.length + B.length;`, and `int half = (total + 1) / 2`
		- we `+ 1` to make sure the median is always on the left partition
		- If total is `10`, then `10 + 15/ 2 = 6` (L and R has same # of elems). If total is `11`, then `11 + 1 / 2 = 6` (L has 6, R has 5). 
			- Both cases, L is either same or larger than R, so median will always be in L
	- `int l = 0` and `int r = A.length`
		- We do `r = A.length` and not `A.length - 1` because we want to allow the possibility that ALL elements of A are smaller than the median (they are all Left partition and NO right partition)
		- If `Bleft > Aright: l = i + 1` (the else case)
			- (Tracing example with `A = [1,2], B = [3,4,5,6]`)
			- `l` can reach up to `r`, then `i = (1+r)/2` can be exactly `A.length`, which means all of `A` is in the left partition
	- The `Integer.MIN_VALUE` and `Integer.MAX_VALUE`
		- We give the `MIN_VALUE` to the lefts and the `MAX_VALUE` to the rights because if they are out of bounds, when we compare the check we want it to pass
- Time Complexity
	- The problem states that the solution must run $O(\log(m+n))$ time.
	- But 