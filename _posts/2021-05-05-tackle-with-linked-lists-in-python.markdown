---
layout: post
title:  "Tackle with linked lists in Python"
author: Wu, Lei
date:   2020-05-05 08:00:00 +0300
categories: [computer science, data structure]
tags: [linked lists, python]
---

Linked list is one of the most fundamental data structures in the world of computer science. This article discusses the concepts related to linked lists, how to implement and manipulate them in Python and demostrates how it can be applied to build more complex data structures and solve problems.

## What are linked lists?

Linked list is a sequence of nodes that are linked one by one in order by reference of neighboring node(s). Specifically, the more accurate definitions of various linked lists are given as the following.

- Singly linked lists

    Singly linked list is a data structure that contains a sequence of nodes such that each node contains an object and a reference to the next node in the list.
    The first node is referred to as the **head** and the last node is referred to as the **tail**; the tail's next field is null. Sometimes, a sentinel node or a self-loop can be used instead of null to mark the end of the list.

![Singly linked lists](/serpent-slayer/assets/images/singly-linked-list.png)

- Doubly linked lists

    In a Doubly linked list, each node has a link to its predecessor.
    
- Circularly linked lists

    Comparing with singly linked lists, tail of a circularly linked list uses its next reference to point back to the head of the list.

The **key attribute** of a linked list is that inserting and deleting elements in a list has time complexity *O(1)*. On the other hand, searching to obtain the *k*-th element in a list is expensive, having *O(n)* time complexity.

## How it works?

### Implementation

Here below is probably the simplest prototype implementation of a singly linked list in Python, where an instance of the `ListNode` type can be used to represent the starting node, thus the whole linked list. For each of the list node, the `next` field contains the reference to the next node.

```python
class ListNode:
  def __init__(self , data=0, next_node=None)
    self.data = data
    self.next = next_node
```

Of course, here the `data` field can be customized at will for different applications.

### Basic operations

The most common and basic operations related to linked lists include search for a key in the list, insert and delete a node after a specific node. Examples of those are given below.

```python
# search for a key
def search(L: ListNode, key: int):
    while L and L.data != key:
        L = L.next
    return L

# insert after a node
def insert_after(node: ListNode, inserted: ListNode):
    inserted.next = node.next
    node.next = inserted

# delete after a node
def delete_after(node: ListNode):
    if node.next:
        node.next = node.next.next
```

## Manipulating linked lists


### Sentinel

### Two iteratrors

### Cycles & overlappings

### Sorting

## Applications of linked lists

### Stacks & queues

### Deque

### ...

## Discuss



