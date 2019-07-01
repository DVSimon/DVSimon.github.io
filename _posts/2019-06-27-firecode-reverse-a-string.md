---
title: "Firecode 6: Reverse a String"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Write a function that takes in a string and returns the reversed version of the string.

Examples:

reverse_string("abcde") -> "edcba"

reverse_string("1") -> "1"

reverse_string("") -> ""

reverse_string("madam") -> "madam"

## My Code

```python
def reverse_string(a_string):
    str_len = len(a_string)
    indexer = -1
    new_str = ""
    for x in range(0,str_len):
        new_str += a_string[indexer]
        indexer -= 1
    return new_str
```

## Explanation

* Use a for loop to iterate over every character of the string and place the characters into a new string.  Since we want the string reverse, we need to start at the end of the string and iterate backwards.
* Start with an index of -1 and go through the length of the string using range() function in conjunction with the length of the string
* Iterate by -1 each time(negative index so -1 gives the next character going backwards) and place the char into new string.
