---
layout: post
title:  "Tackle with linked lists in Python"
author: Wu, Lei
date:   2020-05-09 09:15:00 +0300
categories: [computer science, data structure]
tags: [linked lists, python]
---

Linked list is one of the most fundamental data structures in the world of computer science. This article discusses the concepts related to linked lists, how to implement and manipulate them in Python and demostrates how it can be applied to build more complex data structures and solve problems.

## What are linked lists?

Linked list is a sequence of nodes that are linked one by one in order by reference of neighboring node(s). Specifically, the more accurate definitions of various linked lists are given as the following.

- Singly linked lists

    Singly linked list is a data structure that contains a sequence of nodes such that each node contains an object and a reference to the next node in the list.
    The first node is referred to as the **head** and the last node is referred to as the **tail**; the tail's next field is null. Sometimes, a sentinel node or a self-loop can be used instead of null to mark the end of the list.

![Singly linked lists](/serpent-slayer/assets/images/210509/singly-linked-list.png)

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

Reversing a linked list or a section of it is usually a middle step for a more complex algorithm. An example of reversing the nodes from index `start` to `finish` is given below.

```python
def reverse_sublist(L: ListNode, start: int,
                    finish: int) -> Optional[ListNode]:
    if not L or start <= 0 or finish <= start: 
        return L
    head = before = ListNode()
    head.next = L
    # find the start node
    i = 1
    while i < start:
        before = before.next
        i += 1
    start_node = before.next
    pre, curr = None, start_node
    # reverse
    while i <= finish:
        nxt = curr.next
        curr.next = pre
        pre, curr = curr, nxt
        i += 1
    # connect
    before.next, start_node.next = pre, curr
    return head.next
```

If a complete linked list is to be reversed, the algorithm can be simplified as the following.

```python
def reverse_list(l: ListNode) -> ListNode:
    pre, curr = None, l
    while curr:
        nxt = curr.next
        curr.next = pre
        pre, curr = curr, nxt
    return pre
```

The reversing utility can facilitate solving more complicated problem, like in the example below to decide whether a linked lists is palindromic. The original is first divided into two halves, by using two iterators (which will also be covered a bit later). The second half is then reversed using the function we already have. This way, if the list is palindromic, when iterating elements of both halves of the list, the corresponding elements must match, except optionally one left.

```python
def is_linked_list_a_palindrome(L: ListNode) -> bool:
    # find the middle at slow
    slow, fast = L, L
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next

    first, second = L, reverse_list(slow)
    while first and second:
        if first.data != second.data:
            return False
        first, second = first.next, second.next
    return True
```

### Two iteratrors

Sometimes, having two iterators on a linked list can save the step to traverse the list multiple times. Two iterators moving at the same speed forms a sliding window of fixed size, while on the other hand, a slow and a fast iterators guarentee the distances walked keeps a fixed ratio, e.g. *1: 2*.

The following program deletes the k-th last node in `L`. Two iterators keeping a distance of `k` is used to locate the node to be removed.

```python
# Assumes L has at least k nodes.
def remove_kth_last(L: ListNode, k: int) -> Optional[ListNode]:
    # TODO - you fill in here.
    slow = fast = L
    dummy = pre = ListNode()
    dummy.next = L
    for _ in range(k):
        fast = fast.next
    while fast:
        pre, slow, fast = pre.next, slow.next, fast.next # slow is the target node
    nxt = slow.next
    pre.next = nxt
    return dummy.next
```

### Cycles & overlappings

An iterator that runs twice the speed of another can be used to find cycle in linked list. First, the fast and slow iterators are doom to meet if there is a cycle, as illustrated in the figure below.

![Cycle in a linked list](/serpent-slayer/assets/images/210509/cycle-in-list.png)

Here the trick is to find the starting point of the cycle if it exists. After arriving at the meeting point, it is observed that the distance between list head and the cycle start should be identical to the one between the meeting point to the cycle start, along the path of fast iterator. This way, the code to find a cycle start is given as below.

```python
def has_cycle(head: ListNode) -> Optional[ListNode]:
    # fast and slow iterator
    slow = fast = head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow is fast: # meet
            slow = head
            while slow is not fast:
                slow, fast = slow.next, fast.next
            return slow
    return None
```

When two linked lists are concatenated together, often it is safe to check whether there is a cycle formed, using the same technique above.

Besides cycles, two different linked lists can overlap in later nodes, yet forming another topological structure. Here the key observation is that the tail of the two overlapping lists must be the same node.

```python
def overlapping_no_cycle_lists(l0: ListNode, l1: ListNode) -> ListNode:
    if not l0 or not l1:
        return None
    def get_tail_with_length(l: ListNode):
        if not l:
            return None, 0
        length = 1
        while l.next:
            l = l.next
            length += 1
        return l, length

    t0, len0 = get_tail_with_length(l0)
    t1, len1 = get_tail_with_length(l1)
    if t0 is not t1:
        return None
    
    if len0 > len1:
        l0, l1 = l1, l0
        len0, len1 = len1, len0
    t0, t1 = l0, l1
    for _ in range(len1 - len0):
        t1 = t1.next
    
    while t0 is not t1:
        t0, t1 = t0.next, t1.next
    return t0
```

The above code tries to find the starting node of the overlapping, otherwise returns `None`. A utility for getting the tail and length of a linked list is utilized, to compute the difference of length, if overlapping is observed.

### Sorting

Sorting of a linked list might be implemented via various algorithms, which will be covered andn discussed in later articles specifically for sorting.

## Applications of linked lists

The property of fast insertion and deletion of an element in a sequence makes the linked lists good building blocks for more complicated structures.

### Stacks & queues

As we all know, a **stack** is a data structure that supports Last-In-First-Out (LIFO) operations. Since a linked list always has a reference to its head, and manipulation at the list head is efficient *O(1)* time, it would be natural to orient the top of a stack at the list head. A concrete implementation of stack, utilizing a linked list is given below.

```python
class LinkedStack:
    """LIFO Stack implementation using a singly linked list for storage."""

    # Linked list node subclass
    class ListNode:
        def __init__(self , data=0, next_node=None)
            self._data = data
            self._next = next_node
    
    # Stack related methods
    def __init__(self):
        self._head = None
        self._size = 0

    def __len__(self):
        return self._size

    def is_empty(self):
        return self._size == 0
    
    def push(self, data):
        self._head = self.ListNode(data, self._head)
        self._size += 1

    def pop(self):
        if self.is_empty():
            raise ValueError("Stack empty")
        res = self._head._data
        self._head = self._head._next
        self._size -= 1
        return res
        
    def top(self):
        if self.is_empty():
            raise ValueError("Stack empty")
        return self._head._data
```

A queue being a counterpart of the stack, on the contrary, supports First-In-First-Out (FIFO) operations. A similar implementation utilizing linked lists is given as the following.

```python
class LinkedQueue:
    """FIFO queue implementation using a singly linked list for storage."""

    # Linked list node subclass
    class ListNode:
        def __init__(self , data=0, next_node=None)
            self._data = data
            self._next = next_node

    # Queue related methods
    def __init__(self):
        self._head = None
        self._tail = None
        self._size = 0

    def __len__(self):
        return self._size

    def is_empty(self):
        return self._size == 0

    def first(self):
        if self.is_empty():
            raise ValueError("Queue empty")
        return self._head._data

    def dequeue(self):
        if self.is_empty():
            raise ValueError("Queue empty")
        res = self._head._data
        self._head = self._head._next
        self._size -= 1
        if self.is_empty():
            self._tail = None
        return res

    def enqueue(self, data):
        latest = self.ListNode(data)
        if self.is_empty():
            self._head = latest
        else:
            self._tail.next = latest
        self._tail = latest
        self._size += 1
```

### Deque

### ...

## Discuss

## References



