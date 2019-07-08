---
title: "Firecode 22: Power of 4"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Write a method to determine whether a given integer (zero or positive number) is a power of 4 without using the multiply or divide operator. If it is, return True, else return False.


Example:

```
is_power_of_four(5) ==> False

is_power_of_four(16) ==> True
```

## My Code

```python
def is_power_of_four(number):
    temp = 1
    while temp < number:
        temp = temp << 2
    return temp == number
```

## Explanation

* This one was very tough, had to look up how to use bit manipulation to check powers and bit maniuplation in Python.
* In bit malipulation in Python the shift(for left) multiplies essentially, so shifting over once is multiplying by two, shifting over twice is multiplying by four and so forth(2 * the number of times we shift left).
* We can use the shift then to mimic power of fours by shifting a temp variable starting with 1 over twice nonstop till its bigger or equal to input number(1->4->16->64...)
* If the resulting number is == to input number, it was divisible!
