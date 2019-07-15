---
title: "Algoexpert: Insertion Sort"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def insertionSort(array):
    # Write your code here.
	# for i in range(1, len(array)):
	# 	j = i-1
	# 	current = array[i]
	# 	while current < array[j] and j>=0:
	# 		array[j+1] = array[j]
	# 		array[j] = current
	# 		j -=1
	# return array

	for i in range(1, len(array)):
		j = i
		while array[j] < array[j-1] and j > 0:
			array[j], array[j-1] = array[j-1], array[j]
			j-=1
	return array

```

## Explanation

* Insertion sort uses a partly sorted subarray and unsort subarray of the overall array and uses a sort of bookmark to seperate the two.
* First use a loop starting from 1(0th element is already sorted and j-1 prevents needing to start at 0) to the length of the array.
* Use a temporary variable to compare the element at the current iteration of the loop with all of it's predecessors in the sorted portion until it's larger than one or at the bottom of the sorted array, this is done by moving the temporary variable(j) down 1 every time and continuing to swap the former ith element over.
* After the nested loop finishes, the key moves over(i increments) and the process continues.
