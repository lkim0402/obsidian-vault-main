- You are given an array of k linked lists lists, where each list is sorted in ascending order.
- Return the sorted linked list that is the result of merging all of the individual linked lists.
- https://neetcode.io/problems/merge-k-sorted-linked-lists/question
# solution
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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0 || lists == null) return null;
        
        while (lists.length > 1) {
            List<ListNode> mergedLists = new ArrayList<>();
            for (int i = 0; i < lists.length; i += 2) {
                ListNode l1 = lists[i];
                ListNode l2 = (i + 1 < lists.length) ? lists[i + 1] : null;
                mergedLists.add(merge(l1, l2));
            }
            lists = mergedLists.toArray(new ListNode[0]);
        }
        return lists[0];
    }

    public ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            } else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        if (l1 != null) {
            cur.next = l1;
        } else {
            cur.next = l2;
        }
        return dummy.next;
    }
}
```

- the concept
	- get the `lists` that has the linkedlists
	- merge them by two, until the length of the `lists` is 1
- example
	- `lists = [L1,L2,L3,L4,L5]`
	- in the while loop
		- `lists = [L1+L2, L3+L4, L5]` (by `+`, it means they are sorted & merged, so `L1,L2` and `L3,L4` are sorted & merged)
		- `lists = [L1+L2+L3+L4, L5]`
		- `lists = [L1+L2+L3+L4]`
- `lists = mergedLists.toArray(new ListNode[0]);`
	- you make the arraylist to an array using `toArray`
	- we pass in `new ListNode[0]` because by default, `toArray` converts to `Object[]` (u lose type information)
		- this provides the *type*
		- this provides the *allocation* - by passing an empty array of size `0`, the `toArray` method will allocate a new array of the same type if the array passed in is too small to hold the list elements
- `merge`
	- this was on leetcode easy
# time complexity
- `O(N log k)`
	- `k` = The number of linked lists (the length of the lists array).
	- `N` = The total number of nodes across all lists combined.
- `log k` (height, or the levels)
	- The number of times you can divide `k` by 2 until you reach 1 is `log k`
- `N` (work per level)
	- The work per level is `N`
	- In every single pass, you process every node to merge them