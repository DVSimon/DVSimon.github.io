---
title: "Firecode 7: Repeated Elements in Array"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Write a function - duplicate_items to find the redundant or repeated items in a list and return them in sorted order.
This method should return a list of redundant integers in ascending sorted order (as illustrated below).

Examples:
duplicate_items([1, 3, 4, 2, 1]) => [1]

duplicate_items([1, 3, 4, 2, 1, 2, 4]) => [1, 2, 4]


## My Code

```python
def duplicate_items(list_numbers):
    list_numbers.sort()
    print(list_numbers)
    dupe_numbers = []
    for idx, number in enumerate(list_numbers):
        try:
            if number == list_numbers[idx+1]:
                dupe_numbers.append(number)
        except IndexError:
            break
    return dupe_numbers
```

## Explanation

* Sort the elements first so duplicates come directly after each other.
* Create new empty list for duplicates to be stored in.
* Check each number in list and see if following element in list is equivalent, if it is, place it in the dupe list.
* Use try and except to avoid indexing error for doing last index + 1 comparison.
