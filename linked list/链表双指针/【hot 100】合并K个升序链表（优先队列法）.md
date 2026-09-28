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
import sys 
import heapq


# ListNode类
class ListNode:
    def __init__(self, val = 0, next = None):
        self.val = val
        self.next = next

    # # heapq 在比较堆里的元素时，会调用元素的<运算符
    # 如果你的堆里直接放 ListNode 对象，Python 不知道怎么比较两个节点谁大谁小，就会报错
    # __lt__ 就是告诉 Python「按 val 比大小」
    def __lt__(self, other): 
            return self.val < other.val

# 核心函数   
class Solution:
    def mergeKLists(self, lists):
        if not lists:
            return None

        dummy = ListNode(-1)
        p = dummy # 首先建立结果，因为未知头结点，使用dummy

        pq = [] # 建立最小堆
        for i, head in enumerate(lists):
            if head is not None: # heappush(堆,(要插入的元素))
                heapq.heappush(pq, (head.val, i, head)) # 元组比较，先比较节点值、都一样的话比较索引排序；head方便接下去

        while pq:
            val, i, node = heapq.heappop(pq)
            p.next = node
            if node.next is not None:
                heapq.heappush(pq, (node.next.val, i, node.next))
            p = p.next

        return dummy.next


# 列表->链表
def build_list(array):
    dummy = ListNode(-1)
    p = dummy
    for num in array:
        p.next = ListNode(num)
        p = p.next
    return dummy.next
        
    
# 链表打印成str数组
def print_list(head):
    vals = []
    p = head
    while p:
        vals.append(str(p.val))
        p = p.next
    return (len(vals), " ".join(vals))


# 主函数
def main():
    data = sys.stdin.read().strip().split("\n")
    idx = 0
    while idx < len(data): # 一定要这个，处理多组情况
        k = int(data[idx]); idx += 1
        lists = []
        for _ in range(k):
            parts = list(map(int, data[idx].split())); idx += 1
            n = parts[0] # 这条链表的长度
            nums = parts[1: 1 + n]
            lists.append(build_list(nums))
        
        merged = Solution().mergeKLists(lists)
        print(*print_list(merged)) # 解包运算符
    
if __name__ == "__main__":
    main()
```
PS：解包运算符：ACM 里为什么常用 print(*arr)

因为 ACM 题目输出经常要求一行空格分隔的数字，print(*arr) 一行搞定：

```python
print(*list_to_array(merged))
# 输出：1 1 2 3 4 4 5 6
如果题目要求别的格式：
逗号分隔：print(*arr, sep=",")
每个元素单独一行：print(*arr, sep="\n") 或循环 print(x)
带方括号逗号（像 Python 列表）：print(arr) 或 print("[" + ",".join(map(str, arr)) + "]")
```

# Complexity Analysis
- 该算法的时间复杂度相当于是把 k 条链表分别遍历 O(logk) 次。那么假设 k 条链表的元素总数是 N，该算法的时间复杂度就是 O(Nlogk)
- 该算法的空间复杂度只有递归树堆栈的开销，也就是 O(logk)
