- https://neetcode.io/problems/permutation-string/solution
# my attempt
```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        /*
        - make a hashmap out of s1 (key = char, value = count)
          - "abcc" -> {"a" : 1, "b" : 1, "c" : 2}
        - sliding window
          - size is s1.length()
          - make a hashmap per iteration and compare to s1's map
        */

        Map<Character, Integer> s1Map = new HashMap<>();
        for (Character c : s1.toCharArray()) {
            s1Map.put(c , s1Map.getOrDefault(c, 0) + 1);
        }

        // sliding window
        int r = s1.length() - 1;
        int l = 0;
        while (r < s2.length()) {
            String substring = s2.substring(l,r+1);
            System.out.println(substring);
            Map<Character, Integer> map = new HashMap<>();
            for (Character c : substring.toCharArray()) {
                map.put(c , map.getOrDefault(c, 0) + 1);
            }
            if (map.equals(s1Map)) return true;
            l++;
            r++;
        }
        return false;        
    }
}
```
- since we're making a new hashmap all the time, this is kinda inefficient
- we can just remove the old value and add a new value
- time complexity
	- `O(N * M)`
	- `N` = `s2.length()`
	- `M` = `s1.length()`
# solution - using `int[26]` + sliding window

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()) return false;

        int[] s1Count = new int[26];
        int[] s2Count = new int[26];
        for (int i = 0; i < s1.length(); i++) {
            s1Count[s1.charAt(i) - 'a']++;
            s2Count[s2.charAt(i) - 'a']++;
        }

        int matches = 0;
        for (int i = 0; i < 26; i++) {
            if (s1Count[i] == s2Count[i]) {
                matches++;
            }
        }

		for (int r = s1.length(), l = 0; r < s2.length(); r++, l++) {
            if (matches == 26) return true;

			// Add new character (r)
            int index = s2.charAt(r) - 'a';
            s2Count[index]++;
            if (s1Count[index] == s2Count[index]) {
                matches++;
            } else if (s1Count[index] + 1 == s2Count[index]) {
                matches--;
            }
			// Remove old character (l)
            index = s2.charAt(l) - 'a';
            s2Count[index]--;
            if (s1Count[index] == s2Count[index]) {
                matches++;
            } else if (s1Count[index] - 1 == s2Count[index]) {
                matches--;
            }
        }
        
        // Check the very last window after the loop finishes
        return matches == 26;
    }
}

```

- use array instead of hashmap
- the last for loop
	- `l` -> starts at the beginning
	- `r` -> starts at the index *after* we looped through`s1`, so `s1.length()`
- `s1Count[index] + 1 == s2Count[index]`
	- we check if WE MUST HAVE BEEN EXACTLY EQUAL A MOMENT AGO
	- by adding 1 with the current `index`, we broke a perfect match!!
	- ex)  `s1Count[index] == 2` and `s2Count[index] == 2` initially. now, it's `s2Count[index] == 3`. so since we added 1, it's exactly 1 difference from before, breaking the match.

# time complexity
- `O(N)`
	- `N` = length of `s2`
- breakdown
	1. **Initialization ($O(M)$):**
	    - You iterate through `s1` (length $M$) once to build the initial frequency counts.
	2. **Initial Match Count ($O(1)$):**
	    - You iterate exactly 26 times to check the initial `matches` score. This is constant time.
	3. **Sliding Window ($O(N)$):**
	    - The `for` loop runs from index $M$ to $N$. This is approximately $N$ iterations.
	    - **Inside the loop:** Every operation is $O(1)$.
	        - `charAt`: Constant time lookup.
	        - `++` / `--`: Constant time math.
	        - `if` checks: Constant time comparisons.
    - Crucially, **there is no inner loop** (like looping through the 26 characters) and **no string generation** (like `substring`).
- **Total:** $O(M) + O(N) = O(N)$ (Since $N \ge M$).