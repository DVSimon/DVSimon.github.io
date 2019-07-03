---
title: "Firecode 13: Max Gain"
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
```
```
max_gain([0,50,10,100,30]) ==> 100
```


## My Code

```python
def max_gain(input_list):
    list_len = len(input_list)-1
    max_gain = 0
    for i in range(list_len):
        for j in range(i+1, list_len):
            temp_gain = input_list[j] - input_list[i]
            if temp_gain > max_gain:
                max_gain = temp_gain
    return max_gain
```

## Explanation

* Use nested for loops to compare the current element in outer loop to every element indexed AFTER the current element.  Thus only elements appearing after element in list can be counted for gain.
* Outer loop traverses every element in list while inner loop traverse every element post outer element.
* max_gain initally at 0, if different > max_gain set max_gain equal to it.
* Finally return max_gain
