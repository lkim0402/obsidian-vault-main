https://school.programmers.co.kr/learn/courses/30/lessons/1844
```java
import java.util.*;

class Solution {
  public static int[] dr = {0,0,1,-1};
  public static int[] dc = {1,-1,0,0};

  public int solution(int[][] maps) {
    int n = maps.length;
    int m = maps[0].length;

    // { x, y, count}
    Queue<int[]> queue = new LinkedList<>();
    queue.add(new int[] {0,0,1});
    boolean[][] visited = new boolean[n][m];
    visited[0][0] = true;

    while (!queue.isEmpty()) {
      int[] current = queue.poll();
      int r = current[0];
      int c = current[1];
      int count = current[2];

      if (r == n - 1 && c == m - 1) return count;

      for (int i = 0; i < 4; i++) {
        int newR = r + dr[i];
        int newC = c + dc[i];
        if (newR >= 0 && newR < n && newC >= 0 && newC < m) {
          if (maps[newR][newC] == 1 && !visited[newR][newC]) {
            queue.add(new int[] {newR, newC, count + 1});
            visited[newR][newC] = true;
          }
        }
      }
    }

    return -1;
  }
}
```

# Mistakes
## Java's short circuit evaluation

Wrong:
```java
boolean isInGrid = newR >= 0 && newR < n && newC >= 0 && newC < m;

// If newR is -1, this crashes with ArrayIndexOutOfBoundsException right here.
boolean isVisited = visited[newR][newC]
if (isInGrid && !isVisited) {
  if (maps[newR][newC] == 1) {
	queue.add(new int[] {newR, newC, count + 1});
	visited[newR][newC] = true;
  }
}
```
- This is wrong:
	- You cannot access an array index before verifying it is valid
	- Java executes code line-by-line. By assigning `isVisited` to a variable on a separate line, you force Java to access the array **before** checking if the coordinates are safe.
    - **Short-Circuiting requires `&&`:** The "Short-Circuit" protection only applies inside a single boolean expression (e.g., `A && B`). If `A` is false, `B` is never touched. By separating them into variables, you lose this protection.

Correct:
```java
// Check bounds FIRST. If this fails (false), Java stops immediately.
// It will NEVER attempt to read visited[newR][newC].
if (newR >= 0 && newR < n && newC >= 0 && newC < m) {
  if (maps[newR][newC] == 1 && !visited[newR][newC]) {
	queue.add(new int[] {newR, newC, count + 1});
	visited[newR][newC] = true;
  }
}
```
- You need to make sure that `-1` can never be inside visited

## `count++` and `count + 1`

Wrong:
```java
if (maps[newR][newC] == 1 && !visited[newR][newC]) {
	queue.add(new int[] {newR, newC, count++});
	visited[newR][newC] = true;
}
```
- This is wrong:
	- count++ modifies the variable itself, which ruins the calculation for other neighbors
	- **Post-Increment returns the OLD value:** `count++` gives the value _before_ the increase. If `count` is 5, you add `{..., 5}` to the queue (the distance didn't increase)
	- **Variable Mutation:** `count++` permanently changes the `count` variable in memory. This affects the **next** neighbor in the same loop.
- **Scenario:** Imagine you are at a square with `count = 5` and you have two valid neighbors (Up and Right).
	- **Loop 1 (Neighbor Up):**
	    - You use `count++`.
	    - Queue receives `5` (Wrong, should be 6).
	    - `count` becomes `6` in memory.
	- **Loop 2 (Neighbor Right):**
	    - You use `count++` again.
	    - Queue receives `6` (Wrong, it should be equal to Neighbor Up).
	    - `count` becomes `7`.

Correct:
```java
// Uses the value (5 + 1) = 6
// Does NOT change the 'count' variable, so it stays 5 for the next neighbor.
queue.add(new int[] {newR, newC, count + 1});
```