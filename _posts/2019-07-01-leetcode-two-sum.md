---
title: "Leetcode - Two Sum"
categories:
  - Leetcode
tags:
  - leetcode
  - python
---
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```



## My Code

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        inputted = {}

        for idx, number in enumerate(nums):
            lookup_val = target - number
            if lookup_val in inputted:
                return [inputted[lookup_val], idx]
            else:
                inputted[number] = idx
```

## Explanation

* Used brute force originally but ran into time issues, for long input list using brute force with two nested for loops in too complex. O(n**2)
* Switch to dictionary lookup, because dictionaries in Python are like hash tables so O(1) complexity for it but the for loop make its O(n) complexity.
* For each number in the list, check if the target-number is in the dictionary already, if it is then return indices/list with current index and index of number in dict.
* Else add the index to the dictionary using the number as the key.

## Result

Runtime: 32 ms, faster than 97.34% of Python3 online submissions for Two Sum.
Memory Usage: 14.4 MB, less than 35.89% of Python3 online submissions for Two Sum.
