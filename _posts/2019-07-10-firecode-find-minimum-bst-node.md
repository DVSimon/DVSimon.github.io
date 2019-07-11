---
title: "Firecode 26: Find the Minimum BST Node"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a
Binary Search Tree
, return the node with the minimum data.
Example:

```
4                           
/ \                         
2   8     
 / \                     
5  10                  

Output ==> 2 (TreeNode)
```

## My Code

```python
class BinaryTree:

    def __init__(self, root_node = None):
        # Check out Use Me section to find out Node Structure
        self.root = root_node

    def find_min(self,root):
        # Return element should be of type TreeNode
        if root is None:
            return None
        if root.left_child:
            return self.find_min(root.left_child)
        else:
            return root
```

## Explanation

* Add edge case for if tree is empty return None.
* Since it's a BST every node to left of root(current) node is smaller, so keep traversing left until no more left_childs and return this. Recursive call to function.
