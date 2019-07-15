---
title: "Algoexpert: Selection Sort"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
# def selectionSort(array):
#     # Write your code here.
# 	for i in range(0, len(array)):
# 		smallest = i
# 		for j in range(i+1, len(array)):
# 			if array[j] < array[smallest]:
# 				smallest = j
# 		array[i], array[smallest] = array[smallest], array[i]
# 	return array
def selectionSort(array):
    # Write your code here.
	for i in range(0, len(array)):
		small_index = i
		for j in range(i+1, len(array)):
			if array[j] < array[small_index]:
				small_index = j
		swap(i, small_index, array)
	return array

def swap(i, j, array):
	array[j], array[i] = array[i], array[j]
```

## Explanation

* Selection sort works by finding the smallest element and moving it to the front of the unsorted portion of the array.  The sorted portion of the array will never need to be shifted because the smallest element from unsorted is constantly being moved to top.
* Nested for loops O(n*2) time complexity, O(1) space.
* Use the nested loop to find the smallest element of the array and then swap the current iteration of the outer loop(which will be the first element of unsorted array) with the smallest element and iterate.  This causes the ith swapped element to become part of sorted portion of array.
