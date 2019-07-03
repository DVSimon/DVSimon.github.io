---
title: "Firecode 9: Merge Integer Ranges"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a sorted list of integer ranges (see Range in Use Me), merge all overlapping ranges.

Note: Check out the Use Me section to get the structure of the Range class.

Example:

```
Input  : [[1,10], [5,8], [8,15]]
Output : [[1,15]]
```



## My Code

```python
def merge_ranges(input_range_list):
    new_range_list = []
    initial = input_range_list[0]

    for range_list in input_range_list:
        if initial.upper_bound >= range_list.lower_bound:
            if initial.upper_bound <= range_list.upper_bound:
                initial.upper_bound = range_list.upper_bound
        else:
            new_range_list.append(initial)
            initial = range_list
    new_range_list.append(initial)
    return new_range_list
```

## Explanation

* Set a temporary variable equal to first list in range of lists
* If the temporary variables upper bound is equal to or greater than lower bound of list in range of lists, check for max of temp variable and list variables upper bounds and choose largest as new temp variables upper bound.
* If the temporary variables upper bound is not <= lower bound then just append temp variable as a new list in range of lists.
* Do final append to append final temp to new_range_list.
