---
title: "Algoexpert: Bubble Sort"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def bubbleSort(array):
    # Write your code here.
	if array is None:
		return None
	# while i in range(0, len(array)):
	# 	bubbleCheck = False
	# 	while j in range(i+1, len(array)):
	bubbleCheck = False
	count = 0
	while bubbleCheck is False:
		bubbleCheck = True
		for i in range(len(array) - 1 - count):
			try:
				if array[i] > array[i+1]:
					# temp = array[i]
					# array[i] = array[i+1]
					# array[i+1] = temp
					array[i], array[i+1] = array[i+1], array[i]
					bubbleCheck = False
			except IndexError:
				continue
	return array
```

## Explanation

* Use a checker to verify the entire array is traversed without a swap as per bubble sort algorithm.
* Loop through array with nested loop swapping number at array[i], array[i+1] if needed
* Use a counter to optimize, after every loop of outer loop we know the outer number must be the largest so we don't need to check it.
