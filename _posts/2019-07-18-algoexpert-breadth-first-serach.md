---
title: "Algoexpert: Breadth-first Search"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
# Do not edit the class below except
# for the breadthFirstSearch method.
# Feel free to add new properties
# and methods to the class.
class Node:
    def __init__(self, name):
        self.children = []
        self.name = name

    def addChild(self, name):
        self.children.append(Node(name))
        return self

    def breadthFirstSearch(self, array):
        # Write your code here.
		queue = [self]
		while queue:
			node = queue.pop(0)
			array.append(node.name)
			queue.extend(node.children)
		return array
```

## Explanation

### Method 1
* Create a queue for BFS with current(root) node inside.
* Loop over queue until empty each time popping the first element and appending to array.
* append all child nodes from the popped node to the queue to allow for BFS.
