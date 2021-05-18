---
layout: post
title:  "Tackle with binary trees in Python"
author: Wu, Lei
date:   2021-05-16 09:25:00 +0300
categories: [computer science, data structure]
tags: [binary trees, python]
---

As one of the series of articles about data structures and algorithms, this one discusses the basic data structure binary trees, regarding its relevant concepts, properties, common implementation and manipulation techniques. Binary trees often serve as building block for more complex algorithms to solve various problems. Programming language is in Python context as before.

## What is a binary tree?

Formally, a binary has a recursive definition, namely, it is either empty or a root node *r* together with a left binary tree and a right binary tree. As illustrated in the figure (from [Wikipedia - Binary trees](https://en.wikipedia.org/wiki/Binary_tree)) below, it naturally forms a sort of hierarchy.

![A binary tree](/serpent-slayer/assets/images/210516/binary-tree.svg)

Observe that for any node there exists a unique sequence of nodes from the root to that node with each node in the sequence being a child of the previous node. This sequence is sometimes referred to as the **search path** from the root of the tree to the node.

The **depth** of a node *n* is the number of nodes on the search path from the root to n, not including *n* itself. The **height** of a binary tree can be defined as the maximum depth of any node in that tree, in some context, the definition varies with an offset of 1, including the root of the tree. A **level** at a tress is all nodes at the same depth.

A node that has no descendants except for itself is called a **leaf**, like the node with value *11* in the figure. For example, to get all the leaf nodes out of a binary tree, in the order left-to-right, the following recursive code would nail that.

```python
def leaves(t: TreeNode) -> List[TreeNode]:
  if not t:
    return []
  if not t.left and not t.right:
    return [t]
  return leaves(t.left) + leaves(t.right)
```

### Types

- Full binary tree
- Compelete ...
- Perfect ...
- Balanced ...

### Traverse

- Inorder
- Preorder, postorder

## How does it work

### Implementation

#### Linked structure

#### Array-based structure

#### With parent field

9.4

### Recursion

9.1, 9.2

### Manipulation and common techniques

#### Algorithms for balanced trees

#### Traverse

- Preorder and postorder
  9.11, 9.12
- Inorder 
  9.10
- BFS
- Euler tour

## Applications of binary trees

### Expression trees

## Discuss

General trees vs binary trees vs graphs

## References

[Data structures and algorithms in Python](https://www.amazon.com/Structures-Algorithms-Python-Michael-Goodrich/dp/1118290275)

