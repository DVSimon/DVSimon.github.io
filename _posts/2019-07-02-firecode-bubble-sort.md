---
title: "Firecode 12: Bubble Sort"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Write a function that takes in a list of ints and uses
the Bubble Sort algorithm to sort the list 'in place' in ascending order. The method should return the same, in-place sorted list.
Note: Bubble sort is one of the most inefficient ways to sort a large list of integers. Nevertheless, it is an interview favorite.
Bubble sort has a time complexity of O(n2). However, if the
sample size is small, bubble sort provides a simple implementation of a classic sorting algorithm.

Example:

```
bubble_sort([5, 4, 3]) -> [3, 4, 5]
```
```
bubble_sort([3]) -> [3]
```
```

bubble_sort([]) -> []

```


## My Code

```python
def bubble_sort(a_list):
    no_swaps = True
    while no_swaps is True:
        no_swaps = False
        for index, number in enumerate(a_list):
            try:
                if a_list[index] > a_list[index+1]:
                    temp = a_list[index+1]
                    a_list[index+1] = a_list[index]
                    a_list[index] = temp
                    no_swaps = True
            except IndexError:
                continue
    return a_list
```

## Explanation

* Bubble sort swaps elements in list traversing to sort until a whole pass through can be done without needing a swap(nested for loopez O(n**2) complexity).
* Set inital no_swaps to true and outer loop through till a swap isnt done for an entire traversing of the list.
* Traverse the list in a nested inner loop and do swaps, if a swap is done set no_swaps = True so that outer_loop knows to run again, if no swap is done don't change.
* Eventually entire for loop will run without swapping and no_swap will trigger to exit while loop and bubble sort is finished.
