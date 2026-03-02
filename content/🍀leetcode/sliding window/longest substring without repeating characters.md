- Given a string `s`, find the length of the longest substring without duplicate characters.
- A substring is a contiguous sequence of characters within a string.
- Example 1: `Input: s = "zxyzxyz"`
	- Output: `3`
# my solution
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashSet<Character> set = new HashSet<>();
        int maxLen = 0;
        int A = 0;
        int B = 0;
        while (B < s.length()) {
            if (!set.contains(s.charAt(B))) {
                set.add(s.charAt(B));
                maxLen = Math.max(maxLen, B - A + 1);
                B++;
            }
            else {
                set.remove(s.charAt(A));
                A++;
            }
        }
        return maxLen;
    }
}
```
- In the else block
	- we remove `A` until we have no more duplicates left (until `s.charAt(B)` is no longer is in the set)
- we use a hashset *because `contains()/remove()` is `O(1)`*
	- the fact that it stores unique characters is not that important!
	- if we were to use a list the solution will still work, but `contains()` and `remove()` will be `O(N)` for a list
# neetcode's solution
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashSet<Character> set = new HashSet<>();
        int maxLen = 0;
        int A = 0;
        for (int B = 0; B < s.length(); B++) {
            while (set.contains(s.charAt(B))) {
                set.remove(s.charAt(A));
                A++;
            }
            set.add(s.charAt(B));
            maxLen = Math.max(maxLen, B - A + 1);
        }
        return maxLen;
    }
}
```

# time complexity
- `O(N)`
	- both pointers only move forward, the total number of steps is roughly `2N`