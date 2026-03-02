Reverse nodes between indices

My solution:
```java
public void reverseBetween(int m, int n) { 
	if (length <= 1) {
		return;
	}
	
	Node left;
	if (m == 0) {
		left = new Node(0); // dummy
		left.next = head;
	} else {
		int start = 0;
		left = head;
		while (start < m - 1) {
			left = left.next;
			start++;
		}
	}
	
	Node prev = left;
	Node cur = prev.next;
	
	int reverseTimes = n - m + 1;
	while (reverseTimes > 0 && cur != null) {
		Node curNext = cur.next;
		cur.next = prev;
		prev = cur;
		cur = curNext;
		reverseTimes--;
		System.out.println("length: " + length + ", reverseTimes: " + reverseTimes);
	}
	
	if (m == 0) {
		head = prev;
		Node last = left.next;
		last.next = cur;
	} else {
		Node end = left.next;
		end.next = cur;
		left.next = prev;
	}
}
```

Better solution:
```java
public void reverseBetween(int m, int n) { 
	if (length <= 1) {
		return;
	}
	
	Node dummyNode = new Node(0);
	dummyNode.next = head;
	Node prev = dummyNode;
	
	// moving the prev Node
	for (int i = 0; i < m; i++) {
		prev = prev.next;
	}
	Node cur = prev.next;
	
	for (int i = 0; i < n - m; i++) {
		Node toMove = cur.next;
		cur.next = toMove.next;
		toMove.next = prev.next;
		prev.next = toMove;
	}
	
	head = dummyNode.next;
}
```
- This is better because
	- It just moves the nodes backwards