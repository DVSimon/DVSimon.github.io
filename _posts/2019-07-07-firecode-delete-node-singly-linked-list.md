---
title: "Firecode 24: Delete the Node at a Particular Position in a Linked List"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a Singly Linked-List, implement a method to delete the node that contains the same data as the input data.


Example:

```
delete(1->2->3->4,3) ==> 1->2->4
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

    #method for deleting a node having a certain data        
    def delete(self,data):
        current = self.head
        temp = None
        while current.getData() != data:
            temp = current
            current = current.getNext()

        if current.getNext():
            if current == self.head:
                temp = current.getNext()
                self.head = temp
            else:
                temp.setNext(current.getNext())
        else:
            if current == self.head:
                self.head = None
            else:
                temp.setNext(None)

```

## Explanation

* Traverse through linked list till current nodes data equals input data while keeping a temp variable of the previous node.
* If there exists a next node for the current node AND CURRENT IS NOT THE HEAD then set the previous nodes next value equal to the current nodes next value thus erasing the current node from the list.  If there exists a next value and the current node is the head, just set the head equal to the next node thus erasing current node.
* If a next node doesn't exist, check if the current node is the head(1 element list), if it is simply change head to None for empty list.
* Otherwise set the previous nodes next value to none indicating the end of the list.
