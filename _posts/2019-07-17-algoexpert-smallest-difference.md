---
title: "Algoexpert: Smallest Difference"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def smallestDifference(arrayOne, arrayTwo):
    # Write your code here.

	# #method 1 brute force
	# lowest_difference = float("inf")
	# #O(n^2) complexity, O(1) space
	# for numberOne in range(0, len(arrayOne)):
	# 	for numberTwo in range(0, len(arrayTwo)):
	# 		if abs(arrayOne[numberOne]-arrayTwo[numberTwo]) == 0:
	# 			return [arrayOne[numberOne], arrayTwo[numberTwo]]
	# 		elif abs(arrayOne[numberOne]-arrayTwo[numberTwo]) < lowest_difference:
	# 			lowest_difference = abs(arrayOne[numberOne]-arrayTwo[numberTwo])
	# 			lowest_numbers = [arrayOne[numberOne], arrayTwo[numberTwo]]
	# return lowest_numbers

	#method 2
	arrayOne.sort()
	arrayTwo.sort()
	i = 0
	j = 0
	numberOne = arrayOne[i]
	numberTwo = arrayTwo[j]
	arrayOneSize = len(arrayOne) - 1
	arrayTwoSize = len(arrayTwo) - 1
	temp_min  = float("inf")
	while numberOne is not None and numberTwo is not None:
		# tempNumberOne, tempNumberTwo = numberOne, numberTwo
		if abs(numberOne - numberTwo) == 0:
			return [numberOne, numberTwo]
		if abs(numberOne - numberTwo) < temp_min:
			temp_min = abs(numberOne - numberTwo)
			min_array = [numberOne, numberTwo]
		elif numberOne > numberTwo:
			j += 1
			if j > arrayTwoSize:
				return min_array
			else:
				numberTwo = arrayTwo[j]
		else:
			i += 1
			if i > arrayOneSize:
				return min_array
			else:
				numberOne = arrayOne[i]



```

## Explanation

### Method 1
* Brute force, O(n*2) complexity loop through both lists twice and check difference at every element

### Method 2
* O(nlogn + mlogm) time complexity due to both lists/arrays being sorted, O(1) space
* Pointer logic itself is O(n + m) time worst case.
* Sort both lists at start, use two pointers at start of each list.
* If difference between pointer values is 0, return the pointer values.
* Otherwise if the first list pointer is >second list, increment second list pointer, and vice versa until 0 is received or one of the lists goes out of index.
* Need to keep smallest overall difference ina temp variable through every loop as the last combo isn't guaranteed to be the smallest difference.
