---
title: "Algoexpert: Palindrome Check"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def isPalindrome(string):
    # Write your code here.
	half_string_length = int(len(string)/2)

	# if string_length % 2 == 1:
	# 	return False
	# elif string[0:(string_length/2)-1] == string[-1:-(string_length/2)]:
	if string[:half_string_length] == string[-1:-half_string_length-1:-1]:
		return True
	else:
		return False


```

## Explanation

* Use python indexing/list slicing to compare first half of string to second half of string in backwards order.
