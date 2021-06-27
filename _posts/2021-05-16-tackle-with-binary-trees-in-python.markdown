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
  A complete binary tree on n nodes has height $$\lfloor log n \rfloor$$.

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
    yield tree.data # or actual processing on a node
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

With both the sequences from an inorder and preorder traversal, it is possible to reconstruct a binary tree. Assuming the binary tree has a unique key for each of the node, the reconstruction can be implemented as the following.

```python
def binary_tree_from_preorder_inorder(preorder: List[int],
                                      inorder: List[int]) -> BinaryTreeNode:
    if not preorder:
        return None
    root = BinaryTreeNode(preorder[0])
    i = inorder.index(root.data)

    l_inorder, r_inorder = inorder[:i], inorder[i + 1:]
    l_preorder, r_preorder = preorder[1:len(l_inorder) + 1], \
        preorder[-len(r_inorder):] if r_inorder else []

    root.left, root.right = binary_tree_from_preorder_inorder(l_preorder, l_inorder), \
        binary_tree_from_preorder_inorder(r_preorder, r_inorder)
    return root
```

Many different binary trees can have the same preorder traversal sequence. However, if an empty children is also marked in the preorder sequence (e.g. $$ [1, 2, null, null, 3, null, null] $$), the reconstruction becomes doable, as the following.
```python
def reconstruct_preorder(preorder: List[int]) -> BinaryTreeNode:
    if not preorder:
        return None
    
    def build_tree(it: iter) -> BinaryTreeNode:
        node_key = next(it)
        if node_key is None:
            return None

        left = build_tree(it)
        right = build_tree(it)
        
        return BinaryTreeNode(node_key, left, right)

    return build_tree(iter(preorder))
```

It is observed that in a sequence from preorder traversal, all nodes of the right subtree comes after the left one. Thus it is natural to build the left subtree, before building the right counterpart, where the edge case of the empty children hints the end of each building process. Note that the tricky part here is to use the iterator or a variable external to the recursive utility function of `build_tree` to keep record of position to be visited.

- Postorder traversal

  Traverse the left subtree, traverse the right subtree, and then visit the root. Pre-processing the subtrees first can be useful before deciding a result at the root of a tree. For instance when a tree node does not have a `parent` field, the following algorithm utilized a postorder approach to find out the lowest common ancestor of two nodes in a tree.

```python
from collections import namedtuple


def lca(tree: BinaryTreeNode, node0: BinaryTreeNode,
        node1: BinaryTreeNode) -> Optional[BinaryTreeNode]:
    # TODO - you fill in here.
    Status = namedtuple('Status', ['include', 'ancestor'])

    def _check(tree: BinaryTreeNode, n0: BinaryTreeNode, n1: BinaryTreeNode) -> Status:
        """work similarly as postorder traversal"""
        if not tree:
            return Status(0, None)

        left = _check(tree.left, n0, n1)
        if left.include == 2:
            return left
        right = _check(tree.right, n0, n1)
        if right.include == 2:
            return right
        
        include = left.include + right.include + int(tree is n0) + int(tree is n1)
        ancestor = tree if include == 2 else None
        return Status(include, ancestor)

    return _check(tree, node0, node1).ancestor
```

- BFS

  Another common approach is to traverse a tree so that we visit all the positions at depth *d* before we visit the positions at depth *d + 1*. Such an algorithm is known as a **Breadth-First Search** (BFS) or traversal.

```python
from collections import deque

def BFS(tree: BinaryTreeNode) -> List[int]:
    res = []
    q = deque([tree])

    while q:
        curr = q.popleft()
        res.append(curr.data)
        if curr.left:
            q.append(curr.left)
        if curr.right:
            q.append(curr.right)
    return res
```

## Manipulation and common techniques

### Recursion

Algorithms involving a binary tree often make use of the recursive nature of the data structure. For example, to check whether a binary tree is symmetric or not, it can be solved by recursively checking whether its left and right subtree mirrors each other.

```python
def is_symmetric(tree: BinaryTreeNode) -> bool:
    if not tree:
        return True

    def mirror(n1: BinaryTreeNode, n2: BinaryTreeNode) -> bool:
        if not n1 and not n2:
            return True
        if not n1 or not n2:
            return False
        if n1.data != n2.data:
            return False
        return mirror(n1.left, n2.right) and mirror(n1.right, n2.left)

    return mirror(tree.left, tree.right)
```

### Algorithms for balanced trees

Many algorithms like search of a key that process on a binary tree have a time complexity of *O(h)*, where *h* is the tree height. This translates into *O(log n)* complexity for balanced trees, but *O(n)* complexity for skewed trees, where *n* is total number of nodes. To check whether a binary tree is balanced, the following algorithm can be used. Here, a binary tree is said to be height-balanced if for each node in the tree, the difference in the height of its left and right subtrees is at most one.

```python
def is_balanced_binary_tree(tree: BinaryTreeNode) -> bool:
    def check_balance(node: BinaryTreeNode) -> (bool, int):
        if not node:
            return True, -1
        
        left_balance, left_height = check_balance(node.left)
        if not left_balance:
            return False, 0

        right_balance, right_height = check_balance(node.right)
        if not right_balance:
            return False, 0

        return abs(left_height - right_height) <= 1, \
            max(left_height, right_height) + 1
    
    res, _ = check_balance(tree)
    return res
```

## Applications of binary trees

Binary trees form the building blocks for a variety of highly efficient algorithm implementations.

### Expression trees

For instance, an arithmetic expression can be represented by a binary tree whose leaves are associated with variables or constants, and whose internal nodes are
associated with one of the operators $$+$$, $$−$$, $$×$$, and $$/$$. (See Figure 8.8.) Each node in such a tree has a value associated with it.

- If a node is leaf, then its value is that of its variable or constant.
- If a node is internal, then its value is defined by applying its operation to the values of its children.

![Expression tree](/serpent-slayer/assets/images/210516/expression-tree.png)

This tree in the figure above represents the expression $$ ((((3 +1)×3)/((9 −5)+2))−((3×(7−4)) +6)) $$.

## Discuss

### General tree

A general tree is an abstract data type that stores elements hierarchically. With the exception of the top element, each element in a tree has a parent element and zero or more children elements.

Formally, we define a tree *T* as a set of nodes storing elements such that the nodes have a parent-child relationship that satisfies the following properties:

- If *T* is nonempty, it has a special node, called the root of *T*, that has no parent.
- Each node *v* of *T* different from the root has a unique parent node *w*; every node with parent *w* is a child of *w*.

### Euler tour

In some cases when traversing a tree (not necessarily a binary tree), we need more of a blending of the approaches, with initial work performed before recurring on subtrees, additional work performed after those recursions, and in the case of a binary tree, work performed between the two possible recursions.

![Euler tour traversal of a general tree](/serpent-slayer/assets/images/210516/euler-tour.png)

The **Euler tour** traversal of a general tree *T* can be informally defined as a “walk” around *T*, where we start by going from the root toward its leftmost child, viewing the edges of T as being “walls” that we always keep to our left, as shown in the figure above (from [Data structures and algorithms in Python](https://www.amazon.com/Structures-Algorithms-Python-Michael-Goodrich/dp/1118290275)).

- A “pre visit” occurs when first reaching the position, that is, when the walk
passes immediately left of the node in our visualization.
- A “post visit” occurs when the walk later proceeds upward from that position,
that is, when the walk passes to the right of the node in our visualization.

The algorithm for such Euler tour is given in pseudo code as below.

```bash
Algorithm eulertour(T, p):
    perform the “pre visit” action for position p
    for each child c in T.children(p) do
        eulertour(T, c)     {recursively tour the subtree rooted at c}
    perform the “post visit” action for position p
```

## References

[Data structures and algorithms in Python](https://www.amazon.com/Structures-Algorithms-Python-Michael-Goodrich/dp/1118290275)
