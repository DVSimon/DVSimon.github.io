---
title: "Firecode 21: Recursive Preorder Traversal"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a binary tree, write a function to recursively traverse it in the preorder manner. Mark a node as visited by adding its data to the list - pre_ordered_list(defined globally at the top of the editor).


Example:

```

   1
  / \
 2   3     ==> 1245367
/ \ / \
4  5 6  7
```

## My Code

```python
pre_ordered_list = []
class BinaryTree:

    def __init__(self, root_data):
        self.data = root_data
        self.left_child = None
        self.right_child = None

    def preorder(self):
        if self.data is None:
            return
        pre_ordered_list.append(self.data)
        if self.get_left_child():
            self.get_left_child().preorder()
        if self.get_right_child():
            self.get_right_child().preorder()


    def get_right_child(self):
        return self.right_child

    def get_left_child(self):
        return self.left_child

    def set_root_val(self, obj):
        self.data = obj

    def get_root_val(self):
        return self.data
```

## Explanation

* Check if node(root at start) is empty, if it is, return nothing, otherwise append current node to list.
* If a left child exists for current node, call the preorder function recursively on it(and so forth).
* Then do same for right child.
