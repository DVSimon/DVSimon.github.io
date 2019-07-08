---
title: "Firecode 23: Make Palindrome"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a string as the input, append characters to it to make it into a palindrome. Return this new palindrome.

Note: If the input is a palindrome then it should be returned as is.


Example:

```
Input  : race
Output : racecar
```

## My Code

```python
def make_palindrome(input):
    length = int((len(input)/2))
    if input[-1:length-1:-1] == input[:length+1]:
        return input
    else:
        return input+input[-2::-1]
```

## Explanation

* Check if it's a palindrome first by comparing the start to middle of string to the back to middle of string reversed.  This is done by extended string slicing, for reversal step back by -1 each time from -1(last ele) to middle(len/2-1).
* If it's a palindrome return, else return the entire input string concatenated with the reverse of the input string omitting the last element(e.g. start from -2).
