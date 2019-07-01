---
title: "Firecode 11: Numbers And Ranges"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a sorted list and an input number as inputs, write a function to return a Range object, consisting of the indices of the first and last occurrences of the input number in the list. Check out the Use Me section to examine the structure of the Range class.

Note: The List can have duplicate numbers. The indices within the Range object should be zero based.

Example:

```
Input List  : [1,2,5,5,8,8,10]
Input Number : 8
Output : [4,5]

Input List  : [1,2,5,5,8,8,10]
Input Number : 2
Output : [1,1]
```



## My Code

```python
def find_range(input_list, input_number):
    lower_bound = None
    upper_bound = None
    for idx, number in enumerate(input_list):
        if number == input_number:
            lower_bound = idx
            break
    for idx, number in enumerate(input_list):
        if number == input_number:
            upper_bound = idx
    return range(lower_bound, upper_bound)
```

## Explanation

* Use a for loop to find the first occurence of number in list then break out of for loop after setting lower bound variable equal to index from enumerate.
* Use a second for loop to check every element in list to find last occurence of number in list and set upper bound equal to index each time.
* Return range of upper and lower bounds.
