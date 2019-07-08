---
title: "Firecode 25: Find the Middle Node of a Singly Linked-List"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a Singly Linked-List, write a function to find and return the middle node of the list. Try and limit the runtime complexity to O(n) and space complexity to O(1).

Example:

```
1 ==> 1

1->2 ==> 1

1->2->3->4 ==> 2

1->2->3->4->5 ==> 3
```

## My Code

```python
class SinglyLinkedList:
    #constructor
    def __init__(self):
        self.head = None

    #method for setting the head of the Linked List
    def setHead(self,head):
        self.head = head

    def find_middle_node(self):
        node_list = []
        current = self.head
        while current is not None:
            node_list.append(current)
            current = current.getNext()
        if len(node_list) % 2 == 0:
            return node_list[((len(node_list)-1)/2)]
        else:
            return node_list[(len(node_list)/2)]
```

## Explanation

* Create a new python list and store every node inside of it, then get the length of the list.  
* If the length of the list/2 has no remainder, return the list[length-1/2] so that we choose the lower value in even cases as per instructions.
* If the length of the list/2 has a remainder, simply return the middle element.
