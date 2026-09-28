# Problem
https://labuladong.online/zh/problem/leetcode/merge-k-sorted-lists/description/


# Problem Description

给你一个链表数组 lists，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，并返回合并后的链表。

数据范围：

k == lists.length

0 <= k <= 10^4

0 <= lists[i].length

-10^4 <= lists[i][j] <= 10^4

lists[i] 按 升序 排列

lists[i].length 的总和不超过 10^4

给你一个链表数组 lists，每个链表都已经按升序排列。请将所有链表合并到一个升序链表中并返回。


# Key Points

合并 K 个升序链表的关键，在于每一步都从 K 个链表的当前头节点中选出最小值，可以使用优先队列（二叉堆）实现，它支持两种核心操作：

插入一个元素、取出优先级最高（或最低）的元素

它不保证整体有序，只保证每次取出的都是当前最值。

二叉堆是优先队列最常见的实现方式，本质是一棵完全二叉树，用数组存储。它分两种：

最小堆：每个父节点 ≤ 子节点，堆顶是全局最小值

最大堆：每个父节点 ≥ 子节点，堆顶是全局最大值

另外，堆可以把时间复杂度从O(k)降到O(logk)，处理一个节点的时间复杂度是O(logk)，N个节点总和就是O(Nlogk)

# Code

## ACM version

```python

```

# Complexity Analysis
- 该算法的时间复杂度相当于是把 k 条链表分别遍历 O(logk) 次。那么假设 k 条链表的元素总数是 N，该算法的时间复杂度就是 O(Nlogk)
- 该算法的空间复杂度只有递归树堆栈的开销，也就是 O(logk)
