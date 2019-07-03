---
title: "Firecode 17: Unique Chars in a String"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Write a function that takes in an input string and returns True if all the characters in the string are unique, False if there is even a single repeated character.

Example:

```
unique_chars_in_string("abcde") -> True
```

## My Code

```python
def unique_chars_in_string(input_string):
    chars = []
    for char in input_string:
        if char in chars:
            return False
        else:
            chars.append(char)
    return True
```

## Explanation

* Add unique chars to a list, if a char from input string is ever found in list return false, else add the char to the list.
* If entire string added to list without finding a dupe, return true. 
