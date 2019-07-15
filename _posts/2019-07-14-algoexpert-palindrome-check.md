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
	# half_string_length = int(len(string)/2)
	# if string[:half_string_length] == string[-1:-half_string_length-1:-1]:
	# 	return True
	# else:
	# 	return False

	left_idx = 0
	right_idx = len(string) - 1
	while left_idx < right_idx:
		if string[left_idx] != string[right_idx]:
			return False
		left_idx += 1
		right_idx -= 1
	return True
```

## Explanation

* Use python indexing/list slicing to compare first half of string to second half of string in backwards order. But this method uses string slicing so O(n*2) complexity, so try pointers for O(n) time complexity and O(1) space.
