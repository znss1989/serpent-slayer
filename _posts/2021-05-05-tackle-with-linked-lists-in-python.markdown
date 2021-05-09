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

Conceptually, linked lists are simple and straightforward, whereas some common techniques can be handy manipulating with the data structure to solve problems efficiently.

### Sentinel

Keeping a reference to a linked lists can become difficult when it is dynamically updated. Sentinel or **dummy head** (tail) comes to the resuce in such situations, so that after updating operations are done, a program can easily return a reference to the complete list.

In the code snippet below, `head` keeps a reference to the resulting linked list, from merging two original sorted one.

```python
def merge_two_sorted_lists(L1: Optional[ListNode],
                           L2: Optional[ListNode]) -> Optional[ListNode]:
    curr = head = ListNode()
    while L1 and L2:
        if L1.data <= L2.data:
            curr.next = L1
            L1 = L1.next
        else:
            curr.next = L2
            L2 = L2.next
        curr = curr.next
    curr.next = L1 or L2
    return head.next
```

As kind of a reverse direction, the following program splits one linked lists into two, according to even and odd position of its elements and concatenates together, where `dummy_even` and `dummy_odd` keeps the references to the two sub-lists, before merging back.

```python
def even_odd_merge(L: ListNode) -> Optional[ListNode]:
    is_even = True
    dummy_even, dummy_odd = ListNode(), ListNode()
    even, odd = dummy_even, dummy_odd

    while L:
        curr = L
        L, curr.next = L.next, None

        if is_even:
            even.next = curr
            even = even.next
        else:
            odd.next = curr
            odd = odd.next
            
        is_even = not is_even

    even.next = dummy_odd.next  
    return dummy_even.next
```

### Reverse

### Two iteratrors

### Cycles & overlappings

### Sorting

## Applications of linked lists

### Stacks & queues

### Deque

### ...

## Discuss



