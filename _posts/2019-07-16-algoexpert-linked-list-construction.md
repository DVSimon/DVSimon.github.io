---
title: "Algoexpert: Caesar Cipher Encryptor"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
# Feel free to add new properties and methods to the class.
class DoublyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def setHead(self, node):
        # Write your code here.
		if self.head is None:
			self.head = node
			self.tail = node
			return
		self.insertBefore(self.head, node)

    def setTail(self, node):
        # Write your code here.
		if self.tail is None:
			self.setHead(node)
			return
		self.insertAfter(self.tail, node)

    def insertBefore(self, node, nodeToInsert):
        # Write your code here.
		if nodeToInsert == self.head and nodeToInsert == self.tail:
			return
		self.remove(nodeToInsert)
		nodeToInsert.prev = node.prev
		nodeToInsert.next = node
		if node.prev is None:
			self.head = nodeToInsert
		else:
			node.prev.next = nodeToInsert
		node.prev = nodeToInsert


    def insertAfter(self, node, nodeToInsert):
        # Write your code here.
		if nodeToInsert == self.head and nodeToInsert == self.tail:
			return
		self.remove(nodeToInsert)
		nodeToInsert.next = node.next
		nodeToInsert.prev = node
		if node.next is None:
			self.tail = nodeToInsert
		else:
			node.next.prev = nodeToInsert
		node.next = nodeToInsert

    def insertAtPosition(self, position, nodeToInsert):
        # Write your code here.
		#if pos=1 we're setting it to be head
		if position == 1:
			self.setHead(nodeToInsert)
			return
		currentPosition = 1
		#edge case node is not none, if position is off list
		#start at head and move to next node till at pos #
		node=self.head
		while currentPosition != position and node is not None:
			node = node.next
			currentPosition += 1
		#if the next node is none, the node to insert should become tail, otherwise insert before that node
		if node is None:
			self.setTail(nodeToInsert)
		else:
			self.insertBefore(node, nodeToInsert)


    def removeNodesWithValue(self, value):
        # Write your code here.
		node = self.head
		while node is not None:
			next_node = node.next
			if node.value == value:
				self.remove(node)
			node = next_node
			# nodeRemoval = node
			# node = node.next
			# if nodeRemoval.value == value:
			# 	self.remove(nodeRemoval)

    def remove(self, node):
        # Write your code here.
		if node == self.head:
			self.head = self.head.next
		if node == self.tail:
			self.tail = self.tail.prev
		if node.prev is not None:
			node.prev.next = node.next
		if node.next is not None:
			node.next.prev = node.prev
		node.next = None
		node.prev = None

    def containsNodeWithValue(self, value):
      # Write your code here.
		node = self.head
		while node is not None and node.value != value:
			node = node.next
		return node is not None
		# while node.value != value:
		# 	if node is None:
		# 		return False
		# 	node = node.next
		# return True
```

## Explanation

### setHead
* if the head is none, we know the LL is empty, initialize tail and head to this and then call insertBefore.

### setTail
* if tail is empty, same concept, so just call setHead then insertAfter

### insertBefore
* If the node we're inserting is the head and tail and itself then no action needs to be performed to avoid infinite loops.
* If the nodes in the LL already, remove it.
* make the node to inserts  previous node equal to the nodes previous node and its next equal to next node its inserting insertBefore
* if the previous node is none, it's the head, otherwise the previous node needs to point next to the insertion nodes

### insertAfter
* similar to insertBefore

### insertAtPosition
* check if position is 1, then its to be setHead
* otherwise loop till you get to numerical position and insert before that nodes
* if the node is none, then the insertion node also needs to be the setTail

### removeNodesWithValue
* if the node is the tail or head, update the values, if the previous is present, point prev to nodes next, vice versa for if next is present
* set the next and prev values to none to remove it from the list

### containsNodeWithValue
* loop through list till finding the node, if its found then return true, otherwise return False

### removeNodesWithValue
* loop through node list checking nodes at each value, if value == node.value then perform remove operation.  keep a seperate temp node to store the next node so that when removing a node the link to next node is still present. 
