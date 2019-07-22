---
title: "Algoexpert: Validate BST"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def validateBst(tree):
    # Write your code here.
	# if tree.left is None and tree.right is None:
	# 	return True
	return treeHelper(tree, float("-inf"), float("inf"))


def treeHelper(tree, minimumValue, maximumValue):
	if tree is None:
		return True
	if tree.value < minimumValue or tree.value >= maximumValue:
		return False
	if tree.left and tree.value < tree.left.value:
		return False
	elif tree.right and tree.value > tree.right.value:
		return False
	else:
		# return validateBst(tree.left) and validateBst(tree.right)
		return treeHelper(tree.left, minimumValue, tree.value) and treeHelper(tree.right, tree.value, maximumValue)
```

## Explanation

### Method 1
* Check if the left tree and right tree child of a node at every node are present, if they are they must be less than or greater than(respectively) the current tree.node.
* Also need to pass in a minimumValue and maximum value, for every node on the left side they must be less than the root node and vice versa for maximum value.  Use a helper function for this with max and min values, and for left side call pass in -inf for min value and tree.value for max.
* For the right side, pass in inf for max value and tree.value for min value.
