---
title: "Algoexpert: Binary Search"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def binarySearch(array, target):
    # Write your code here.
	return binaryHelper(array, target, 0, len(array)-1)

def binaryHelper(array, target, left, right):
	# if left > right:
	# 	return -1
	# middle = (left+right)//2
	# if array[middle] == target:
	# 	return middle
	# elif array[middle] > target:
	# 	return binaryHelper(array, target, left, middle-1)
	# else:
	# 	return binaryHelper(array, target, middle+1, right)

	while left <= right:
		middle = (left+right)//2
		if array[middle] == target:
			return middle
		elif array[middle] > target:
			right = middle - 1
		else:
			left = middle + 1
	return -1
```

## Explanation

* Use a helper function to divide the array in half.  IF done in the main function and array slicing is used to pass array, can't return the original middle index properly for final result. Using a helper function w/ left and right allows to keep original array intact and to find the original middle element within divided subarrays.
* First soln is recursively so O(n) space b/c recursive on call stack.
* Escape clauise, if the element isnt present in the array, it'll hit else case every time and keep adding 1 to the left and eventually left will be > right meaning not present.
* Second soln is iterative so O(1) space.  left<= right because if we get to last case where left and right(and middle) on same element we still need to check if that's the target.
* If we get through entire loop and left > right, then we know element cant be in array, we cleared entire list.
