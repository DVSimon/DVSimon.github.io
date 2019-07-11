---
title: "Firecode 27: Find the Maximum BST Node"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a Binary Search Tree, return the node with the maximum data.
Example:

```

     4   
    / \                       
   2   8     
      / \                     
     5  10                  

Output ==> 10 (TreeNode)
```

## My Code

```python
class BinaryTree:

    def __init__(self, root_node = None):
        # Check out Use Me section to find out Node Structure
        self.root = root_node

    def find_max(self,root):
        # Return element should be of type TreeNode
        if root is None:
            return None
        if root.right_child is None:
            return root
        else:
            return self.find_max(root.right_child)
```

## Explanation

* Add edge case for if tree is empty return None.
* Since it's BST, every node to right should be larger, keep traversing till last node to right and return this node using recursion.
