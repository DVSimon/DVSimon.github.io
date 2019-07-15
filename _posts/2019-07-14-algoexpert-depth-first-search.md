---
title: "Algoexpert: Find three largest"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
# Do not edit the class below except
# for the depthFirstSearch method.
# Feel free to add new properties
# and methods to the class.
class Node:
    def __init__(self, name):
        self.children = []
        self.name = name

    def addChild(self, name):
        self.children.append(Node(name))
        return self

    def depthFirstSearch(self, array):
        # Write your code here.
		array.append(self.name)
		for child in self.children:
			child.depthFirstSearch(array)
		return array
```

## Explanation

* append self/root node to array at start and for each child call the depthFirstSearch recursively using child as the self.
* this will cause branches to be fully traversed from left to right before moving onto next child effectively creating a DFS queue.
