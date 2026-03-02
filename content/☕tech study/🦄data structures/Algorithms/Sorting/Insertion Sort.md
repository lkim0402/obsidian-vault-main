```python
def insertion_sort(A):
	for i in range(1, len(A)):
		for j in range(i - 1, -1, -1): #end is -1 and not 0 since end is exclusive
		# if right smaller than left, swap
			if A[j] > A[j + 1]:
				temp = A[j]
				A[j] = A[j + 1]
				A[j + 1] = temp
		# if not just break from this inner loop
			else:
				break
				
```

