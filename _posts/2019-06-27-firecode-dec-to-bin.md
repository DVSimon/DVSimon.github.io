---
title: "Firecode: Decimal to binary"
categories:
  - Firecode
tags:
  - firecode
  - python
---

## Prompt

Write a function to compute the binary representation of a positive decimal integer. The method should return a string.

Example:
dec_to_bin(6) ==> "110"

dec_to_bin(5) ==> "101"
Note : Do not use in-built bin() function.

## My Code

```python
def dec_to_bin(number):
    if number < 2:
        return str(number)
    else:
        return (dec_to_bin(number/2) + dec_to_bin(number%2))
```

## Explanation

* If the number passed is less than 2 then we can easily get it's binary equivalent by passing back the string representation of it(e.g. 0 or 1).
* Otherwise we need to calculate the binary value of it, which can be done using recursive calls.
* Decimal to binary conversion using the [divide by two method](https://www.electronics-tutorials.ws/binary/bin_2.html).
* Continuously divide by two to get a result and remainder of 1 or 0 until final result equals 0.
* The remainder will each time become a binary digit to be returned.
