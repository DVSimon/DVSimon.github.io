---
title: "Algoexpert: Three Number Sum"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def threeNumberSum(array, targetSum):
    # Write your code here.
	sumList = []
# 	#method 1, brute force, 3 loops O(n^3) complexity
# 	array.sort()
# 	for x in range(0, len(array)-2):
# 		for y in range(x+1, len(array)-1):
# 			for z in range(y+1, len(array)):
# 				if array[x] + array[y] + array[z] == targetSum:
# 					#sort to use ascending order
# 					sumList.append([array[x], array[y], array[z]])
# 	return sumList

	#method 2, use left and right pointers, O(n^2) complexity, two nested loops
	array.sort()
	# to length-2 because the two pointers will occupy the last two slots eventually so nothing else to check past that
	for x in range(0, len(array)-2):
		r = len(array)-1
		j = x+1
		while j < r:
			if array[x] + array[r] + array[j] == targetSum:
				#if we find it, move j and r over, no other combination of x,j and some value could equate to tgt sum
				sumList.append([array[x], array[j], array[r]])
				j+=1
				r-=1
			elif array[x] + array[r] + array[j] < targetSum:
				#check higher number for left pointers
				j += 1
			else:
				#check lower number for right pointers
				r -= 1
	return sumList			
```

## Explanation

### Method 1
* Brute force, O(n^3) complexity, loop over list 3 times with the outer loop going from start to end-2, middle from start+1 to end-1 and inner from start+2 to end.
* Sort at the start so that numbers added in will be in ascending order
* If the sum of the three equate to target then add to 2d array.

### Method 2
* Sort at start so that we can use left and right pointers and number in ascending order for new sublist insertion.
* Loop over array from start to end-2, create temp variables, left at loop index+1 and right at end.
* Loop with constant for loop variable and left pointer increasing if sum < target and right pointer decreasing if sum > target.
* If target is found, add to list and also increment left pointer/decrement right pointer because no other combination of x,j and some value could equate to tgt sum.
