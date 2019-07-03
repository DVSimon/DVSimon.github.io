---
title: "Firecode 17: Recursive Postorder Traversal"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a binary tree, implement a method to populate the list post_ordered_list with the data of the nodes traversed in postorder, recursively.

Example:

```
1
/ \
2   3     ==> 4526731
/ \ / \
4  5 6  7
```

## My Code

```python
def postorder(self):
  # empty tree?
    # if self.get_root_val() is None:
    #     return
    if self.get_left_child():
        self.get_left_child().postorder()
    if self.get_right_child():
        self.get_right_child().postorder()
    post_ordered_list.append(self.data)
```

## Explanation

* Check left node first by checking if it exists then recursively calling postorder for left node child, then do same for right and finally append root data to list.
* This will cause the tree to call the left most node child to left first then left mode node child to right and so forth while not appending self data till each child postorder has finished.
