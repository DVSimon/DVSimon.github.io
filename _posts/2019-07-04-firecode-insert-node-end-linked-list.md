---
title: "Firecode 20: Inserting a Node at the End of a Singly Linked List"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Write a function to insert a node at the end of a Singly Linked-List.
Click Use Me at the top to check the structure of Node.

Example:

```
LinkedList: 1->2 , Head = 1

insertAtEnd(1) ==> 1->2->1

insertAtEnd(2) ==> 1->2->2

insertAtEnd(3) ==> 1->2->3
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

    #method for inserting a new node at the end of a Linked List   
    def insertAtEnd(self,data):
        newNode = Node()
        newNode.setData(data)
        if self.head is None:
            self.head = newNode
        else:
            current = self.head
            while current is not None:
                last = current
                current = current.getNext()

            last.setNext(newNode)
```

## Explanation

* Check if linked list is empty(head is None), if it is just set the new node as the only element AKA head.
* Else iterate through list till reaching the end(None for getNext) and set the previous elements(last variable) setNext as the newNode.
