---
title: "Firecode 8: Repeated Elements in Array"
categories:
  - Firecode
tags:
  - firecode
  - python
---

## Prompt

You are given an m x n 2D image matrix (List of Lists) where each integer represents a pixel. Flip it in-place along its vertical axis.

Example:
Input image :
1 0              
1 0

Modified to :
0 1              
0 1


## My Code

```python
def flip_vertical_axis(matrix):
    rows = len(matrix)-1
    columns = len(matrix[0])-1
    i = 0
    while i<=rows:
        j=0
        while j<=columns/2:
            temp=matrix[i][j]
            matrix[i][j] = matrix[i][columns-j]
            matrix[i][columns-j] = temp
            j +=1
        i+=1
```

## Explanation

* Use len of rows and columns - 1 because of 0th index.
* Traverse matrix using nested loop for rows/columns.
* Do for only half of columns because otherwise it would be flipped back to original place.
* Use a temp to store original matrix position then flip and Iterate.
* Reset j everytime we go to a new row.
