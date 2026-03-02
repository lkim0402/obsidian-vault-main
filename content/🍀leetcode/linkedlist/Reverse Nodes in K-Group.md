- You are given the head of a singly linked list head and a positive integer k.
- You must reverse the first k nodes in the linked list, and then reverse the next k nodes, and so on. If there are fewer than k nodes left, leave the nodes as they are.
# solution (iteration)
- kinda messed me up lol...
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0, head);
        ListNode groupPrev = dummy;

        while (true) {
            ListNode kth = getKth(groupPrev, k);
            if (kth == null) break;
            ListNode groupNext = kth.next;
            // shortcut: so that tail will connect immediately to 
            // next group head
            ListNode prev = kth.next; 
            ListNode cur = groupPrev.next; // head of cur group
            while (cur != groupNext) {
                ListNode tmp = cur.next;
                cur.next = prev;
                prev = cur;
                cur = tmp;
            }
            // ===== updating groupPrev to new head =====
            // groupPrev is pointing to original head (1st iteration)!
            ListNode tmp = groupPrev.next;
            // update dummy.next to new head
            groupPrev.next = kth;
            // update groupPrev to original head, which is 
            // the node before our new group!
            groupPrev = tmp; 
        }
        return dummy.next;
    }

    private ListNode getKth(ListNode cur, int k) {
        while (cur != null && k > 0) {
            cur = cur.next;
            k--;
        }
        return cur;
    }
}
```

# recursion
```java
public class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode cur = head;
        int group = 0;
        while (cur != null && group < k) {
            cur = cur.next;
            group++;
        }

        if (group == k) {
            cur = reverseKGroup(cur, k);
            while (group-- > 0) {
                ListNode tmp = head.next;
                head.next = cur;
                cur = head;
                head = tmp;
            }
            head = cur;
        }
        return head;
    }
}
```
- so in the reversing while loop 
	- `head` -> the next node we want to reverse
	- `cur` -> the "end node" that we will connect `head.next`
- The `head = cur` after the while loop
	- in the end,  `ListNode tmp = head.next` will be the last element (the very first initial `cur` that we got from recursion)
	- and `cur` will be at the beginning
	- so we set `head = cur` to return the head for next iteration (from the recursion).. or we can also just simple return `cur` 

<img

  src="https://res.cloudinary.com/dwa6lkvcr/image/upload/v1769651182/IMG_0610_alueul.png"

  className="w-[50rem]"

/>
- So the `cur` value returned from `reverseKGroup(cur,k)` will be already reversed and be ready for you.
# Time complexity
- `O(N)`
	  - in the first visit, we check each node(`k` steps) to check if there are enough nodes.
	  - Then inside the `if (group == k)` block, the `head` pointer iterates through those same `k` nodes.
	  - So roughly the total operations are `O(2N)`, but it's reduced to `O(N)`.