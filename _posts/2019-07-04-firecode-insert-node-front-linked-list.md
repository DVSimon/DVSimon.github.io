---
title: "Firecode 19: Insert a Node at the Front of a Linked List"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Write a function to insert a node at the front of a Singly Linked-List

Example:

```
LinkedList: 1->2 , Head = 1
insert_at_front(1) ==> 1->1->2
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

    # Method for inserting a new node at the start of a Linked List   
    def insert_at_front(self,data):
        temp = self.head
        newNode = Node()
        newNode.setData(data)
        self.head = newNode
        newNode.setNext(temp)
```

## Explanation

* Set a temp variable as old head to store it and make it next node for the insertion head.
* Create a new node and set it's data equal to input data.
* Set the new node as the new head and it's next node as the temp variable(AKA the old head).
