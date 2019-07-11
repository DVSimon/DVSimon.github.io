---
title: "Firecode 28: Max Gain"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given an list of integers, write a method - max_gain - that returns the maximum gain. Maximum Gain is defined as the maximum difference between 2 elements in a list such that the larger element appears after the smaller element. If no gain is possible, return 0.
Example:

```
max_gain([100,40,20,10]) ==> 0
max_gain([0,50,10,100,30]) ==> 100
```

## My Code

```python
def max_gain(input_list):
    max_gain = 0
    for idx, number in enumerate(input_list):
        for further_number in input_list[idx+1:]:
            if (further_number-number) > max_gain:
                max_gain = further_number-number
    return max_gain
```

## Explanation

* Initialize max_gain to 0 so if no positive difference, it's just 0.
* Use nested for loops to compare each element after the current element to the current element, and if difference between them is greater than current max_gain, set max_gain to that.
* Return max_gain at end.
