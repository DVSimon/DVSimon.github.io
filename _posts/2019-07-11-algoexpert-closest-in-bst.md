---
title: "Algoexpert: Find closest in BST"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def findClosestValueInBst(tree, target):
    # Write your code here.
	if tree is None:
		return None
	# closest = abs(target-tree.value)
	closest = float("inf")
	currentNode = tree
	while currentNode is not None:
		if abs(target - currentNode.value) < abs(target - closest):
			closest = currentNode.value
		if target < currentNode.value:
			#go left
			currentNode = currentNode.left
		elif target > currentNode.value:
			#go right
			currentNode = currentNode.right
		else:
			break
	return closest
```

## Explanation

* Use iterative method.
* Edge case of if tree is empty return None as closest node.
* Set initial closest value to infinite so that first node will become closest in while loop.
* While loop to iterate through tree until currentNode is empty(e.g. no child L/R of current node). If an empty node is reached it can be assumed that the currently calculated closest value must be the closest overall.
* Check if current node is closer to 0 difference between target than the closest node and if so set closest node to current node.
* If currentnode value is > target, go left in tree, else go right.
* If target is == closestnode, break out of loop and return closest node.
