---
title: "Algoexpert: Two Number Sum"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def twoNumberSum(array, targetSum):
    # Write your code here.
	#soln 1
	# int_store = {}
	# for number in array:
	# 	temp_test = targetSum - number
	# 	if temp_test in int_store:
	# 		return [min(number, temp_test), max(number, temp_test)]
	# 	else:
	# 		int_store.add(number)
	# return []
	#soln 2
	left_index = 0
	right_index = -1
	array.sort()
	temp = None
	while temp != targetSum:
		if array[left_index] == array[right_index]:
			return []
		temp = array[left_index] + array[right_index]
		if temp > targetSum:
			right_index -= 1
		elif temp < targetSum:
			left_index += 1
		elif temp == targetSum:
			return [array[left_index], array[right_index]]
```

## Explanation

* First method:
* To find a sum, we can find the difference between the targetSum passed to us and the number we are iterating over in a for loop through array.
* If the difference between them is present in the array, that value can be determined to be second number, and we can pass the min(2 numbers), max(2 numbers) in an array for sorted version.
* Otherwise, add the current number to array and do the same process for next iteration of array traversal.

* Second method:
* Use a left pointer(start at [0]) and right (start at end of list) pointer and sum up the numbers at each pointers index value.
* If sum > target, move right pointer over to lower the value, if sum < target, move left pointer over to raise value.
* Exit case for if the list is traversed and pointers reach each other to return empty list.
* O(nlogn) time, O(1)
