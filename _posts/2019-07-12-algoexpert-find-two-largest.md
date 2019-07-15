---
title: "Algoexpert: Find three largest"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def findThreeLargestNumbers(array):
    # Write your code here.
	largest_three = [float("-inf"), float("-inf"), float("-inf")]
	for number in array:
		if number > largest_three[2]:
			largest_three[0] = largest_three[1]
			largest_three[1] = largest_three[2]
			largest_three[2] = number
		elif number > largest_three[1]:
			largest_three[0] = largest_three[1]
			largest_three[1] = number
		elif number > largest_three[0]:
			largest_three[0] = number
	return largest_three
```

## Explanation

* Initially set array to negative infinities so that new values always enter at the highest point of list when list is "empty".
* Compare number to largest slot in array, then middle then lowest, adjusting and shifting array accordingly every time.
