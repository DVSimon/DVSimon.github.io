---
title: "Leetcode - Reverse Integer"
categories:
  - Leetcode
tags:
  - leetcode
  - python
---
Given a 32-bit signed integer, reverse digits of an integer.

Example:

```
Input: 123
Output: 321
```
```
Input: -123
Output: -321
```
```
Input: 120
Output: 21
```

## My Code

```python
class Solution:
    def reverse(self, x: int) -> int:
        negative_check = None
        number = str(x)
        if number[0] == "-":
            negative_check = True
        else:
            negative_check = False
        digits = len(number)
        reverse = []
        i = -1
        while -i <= digits:
            if number[i] != "-":
                reverse.append(number[i])
            i-=1
        if (abs(int("".join(reverse))) > (2 ** 31 - 1)):
            return 0
        if negative_check is True:
             return -int("".join(reverse))
        else:
            return int("".join(reverse))
```

## Explanation

* Convert integer to string to index each digit individually.
* Check if string starts with "-" meaning number is negative, set negative_check = true else false.
* Create: digits for length of string/digits to iterate over, initial index at -1(python indexing backwards) and empty list for reverse.
* Iterate backwards through string, append negative if neccessary then each digit.
* Check for 32 bit integer overflow by joining list into string then int conversion then absolute value.  (2 ** 31 - 1) because [largest 32 bit integer isnt 2**32 because of sign digit.](https://stackoverflow.com/questions/45528637/checking-integer-overflow-in-python).
* Else return the reversed negative or positive joined string converted to int.

## Result

Runtime: 32 ms, faster than 95.09% of Python3 online submissions for Reverse Integer.
Memory Usage: 13.2 MB, less than 66.28% of Python3 online submissions for Reverse Integer.
