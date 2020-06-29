---
tags:
  - algorithm
  - "algorithm/data structure"
  - interview
created: 2020-05-19T03:45:09.548Z
modified: 2020-05-19T04:36:12.250Z
---

# Basic data structures

## Basics

### Array - 数组

### Linked list - 链表

### Stack - 栈

### Queue - 队列

### Hash table - 哈希表

**Collision resolution**

1. Separate chaining
   ![](https://i.loli.net/2020/05/19/BHQU8hgwKtxo7F5.png)

2. Open addressing
   ![](https://i.loli.net/2020/05/19/tMc3yYPopQWTivu.png)  
   **Resize**
3. Resize
   HashMap.Size >= Capacity \* LoadFactor
   Usually LoadFactor = 0.75 by default
4. Rehash all values

## Tree

### Binary tree

**Full binary tree - 满二叉树**
a binary tree T is full if each node is either a leaf or possesses exactly two childnodes.

**Complete binary tree - 完全二叉树**
...

### Binary search tree 二叉树 - 由链表实现

Self balanced binary search tree

- Red black tree
- AVL tree
- etc

**Traversal**
Type

- DFS
  - Preorder - 前序遍历 DLR (data, left, right)
  - Inorder - 中序遍历 LDR (left, data, right)
  - Postorder - 后序遍历 LRD (left, right, data)
- BFS
  - Level order - 层序遍历

## Binary heap 二叉堆 - 由数组实现

- Binary Heap has to be complete binary tree at all levels except the last level. This is called shape property.
- All nodes are either greater than equal to (**Max-Heap**) or less than equal to (**Min-Heap**) to each of its child nodes. This is called heap property.

![](https://images.cnitblog.com/i/605165/201408/030922489463608.jpg)

- Insert element

![](https://algorithms.tutorialhorizon.com/files/2015/02/Insert-Bubble-Up-Min-Heap.gif)

- Delete element

![](https://i2.wp.com/algorithms.tutorialhorizon.com/files/2015/02/Delete-OR-Extract-Min-from-Heap.gif?resize=450%2C282)

## Priority queue 优先队列 - 由 Binary heap 实现

- 最大优先队列，无论入队顺序如何，都是当前最大的元素优先出队
- 最小优先队列，无论入队顺序如何，都是当前最小的元素优先出队
