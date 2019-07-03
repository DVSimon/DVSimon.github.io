---
title: "Firecode 16: Count the Leaves"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Write a function to find the total number of leaf nodes in a binary tree. A node is described as a leaf node if it doesn't have any children. If there are no leaf nodes, return 0.

Example:

```
1
/ \
2   3     
/ \ / \
4  5 6  7
/ \
8   9     
==> no. of leaves = 5
```

## My Code

```python
class BinaryTree:
    def __init__(self, root_node = None):
            self.root = root_node

    def number_of_leaves(self,root):
        if root is None:
            return 0
        elif (root.right_child is None) and (root.left_child is None):
            return 1
        else:
            return self.number_of_leaves(root.left_child) + self.number_of_leaves(root.right_child)
```

## Explanation

* Use recursion, if the node(root) is none then return 0.
* If both the children are empty return 1 (for current node).
* Otherwise return recursive call to number of leaves for both children added, this will traverse tree recursively for nodes, when empty nodes are hit their returns will be 0 b/c of the first if statement.
