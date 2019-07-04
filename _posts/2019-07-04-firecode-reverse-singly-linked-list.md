---
title: "Firecode 18: Reverse a Singly Linked List"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a singly linked list, write a method to perform In-place reversal.

Example:

```
1->2->3 ==> 3->2->1
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

    def reverse(self):
        current = self.head
        self.head = None

        while current is not None:
            temp = current.getNext()
            current.setNext(self.head)
            self.head = current
            current = temp
```

## Explanation

* Set a new variable equal to the current head(top of list) which will be tail when reversed.
* Set head initially to none, because we will use head for next node and the back of the list should have no next node so it will be none.
* loop until we aren't past list index.
* Set temp variable equal to next node and the current variables next node equal to the head(which should be the past node when reversing, or in the first nodes case, None).
* Set new head to current node and iterate by setting current node = temp which is the next node.
