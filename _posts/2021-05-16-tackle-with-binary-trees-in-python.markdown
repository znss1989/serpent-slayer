---
layout: post
title:  "Tackle with binary trees in Python"
author: Wu, Lei
date:   2021-05-16 09:25:00 +0300
categories: [computer science, data structure]
tags: [binary trees, python]
---

As one of articles that discusses about data structures and algorithms, this particular one talks about binary trees, regarding its relevant concepts, properties, common implementation and manipulation techniques. Binary trees often serve as building block for more complex algorithms. Programming language used here is Python as well.

## What is a binary tree?

Formally, a binary has a recursive definition, namely, it is either empty or a root node *r* together with a left binary tree and a right binary tree. As illustrated in the figure (from [Wikipedia - Binary trees](https://en.wikipedia.org/wiki/Binary_tree)) below, it naturally forms a sort of hierarchy.

![A binary tree](/serpent-slayer/assets/images/210516/binary-tree.svg)

Observe that for any node there exists a unique sequence of nodes from the root to that node with each node in the sequence being a child of the previous node, except the root. This sequence is referred to as the **search path** from the root of the tree to that particular node.

The **depth** of a node *n* is the number of nodes on the search path from the root to n, not including *n* itself. The **height** of a binary tree can be defined as the maximum depth of any node in that tree, in some context, the definition varies with an offset of 1, including the searched node *n*. A **level** at a tress is all the nodes at the same depth.

A node that has no descendants except for itself is called a **leaf**, like the node with value 11 in the figure above. For example, to get all the leaf nodes out of a binary tree, in the order left-to-right, the following recursive code would nail that.

```python
def leaves(t: BinaryTreeNode) -> List[BinaryTreeNode]:
    if not t:
        return []
    if not t.left and not t.right:
        return [t]
    return leaves(t.left) + leaves(t.right)
```

### Special binary trees

There are certain types of binary trees with special shapes, thus are of interest to be discussed.

- Full binary tree

  A *full binary tree* is a binary tree in which every node other than the leaves has two children.

- Perfect binary tree

  A *perfect binary tree* is a full binary tree in which all leaves are at the same depth, and in which every parent has two children.
  A perfect binary tree of height `h` contains exactly $$ 2^{h+1}-1 $$ nodes, of which $$ 2^h $$ are leaves.

- Complete binary tree

  A *complete binary tree* is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.
  A complete binary tree on n nodes has height $logn$.

- Skewed tree

  A *left-skewed* tree is a tree in which no node has a right child; a *right-skewed* tree is a tree in which no node has a left child. In either case, we refer to the binary tree as being skewed.

## How does it work

### Implementation

A prototype implemention using a linked structure is given as below.

```python
class BinaryTreeNode:
    def __init__(self, data=None, left=None, right=None):
        self.data= data
        self.left, self.right = left, right
```

Sometimes the node object definition also includes a parent field (which is null for the root). If that is indeed the case, it can be used to find the lowest common ancestor (LCA) of two nodes in a binary tree, which is given below.

```python
def lca(node0: BinaryTreeNode,
        node1: BinaryTreeNode) -> Optional[BinaryTreeNode]:
    if not node0 or not node1:
        return None

    def get_depth(node: BinaryTreeNode) -> int:
        res = -1
        while node:
            res += 1
            node = node.parent
        return res

    d0, d1 = get_depth(node0), get_depth(node1)
    if d0 > d1:
        node0, node1 = node1, node0
        d0, d1 = d1, d0

    for _ in range(d1 - d0):
        node1 = node1.parent

    while node0 is not node1:
        node0, node1 = node0.parent, node1.parent
    return node0
```

### Basic operations

#### Traverse

A basic operation for a binary tree is to walk through all the nodes in the tree, which is formally called **traversing** the tree. There are several ways that traversing can be conducted.

- Inorder traversal

  Traverse the left subtree, visit the root, then traverse the right subtree.

```python
def traverse(tree: BinaryTreeNode):
    if not tree:
        return
    traverse(tree.left)
    do_something(tree.data) # actual processing on a node
    traverse(tree.right)
```
  
Normally, inorder traversal can be easily realized recursively as above, with implicit space of $$ O(h) $$, where $$ h $$ is the height of the tree. Considering a binary tree where each node has a `parent` field, this space complexity can be further reduced to be constant, as the following.

```python
def inorder_traversal(tree: BinaryTreeNode) -> List[int]:
    res = []
    prev, curr = None, tree

    while curr:
        if curr.parent is prev: # moving down
            if curr.left:
                prev, curr = curr, curr.left
            else:
                res.append(curr.data)
                prev, curr = curr, curr.right or curr.parent
        else: # moving up
            if curr.left is prev:
                res.append(curr.data)
                prev, curr = curr, curr.right or curr.parent
            else:
                prev, curr = curr, curr.parent

    return res
```

- Preorder traversal

  Visit the root, traverse the left subtree, then traverse the right subtree.

- Postorder traversal

  Traverse the left subtree, traverse the right subtree, and then visit the root.



- Preorder and postorder
  9.11, 9.12

- BFS
- Euler tour

## Manipulation and common techniques

### Recursion

9.1, 9.2


### Algorithms for balanced trees

## Applications of binary trees

### Expression trees

## Discuss

General trees vs binary trees vs graphs
