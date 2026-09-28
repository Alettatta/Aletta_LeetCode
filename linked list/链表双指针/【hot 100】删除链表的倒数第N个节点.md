# Problem
https://labuladong.online/zh/problem/leetcode/remove-nth-node-from-end-of-list/description/


# Problem Description
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

数据范围：

链表中结点的数目为 sz

1 <= sz <= 30

0 <= Node.val <= 100

1 <= n <= sz

给你一个链表的头结点 head 和一个整数 n，删除链表的倒数第 n 个结点，并返回删除后的链表头结点。


# Key Points
步骤：找到链表的倒数第N个节点 -> 删除链表中间节点

问题：如何找到单链表的倒数第K个节点？

假设链表有 n 个节点，**倒数第 k 个节点就是正数第 n - k + 1 个节点**，问题是链表需要遍历一遍 O(n) 才能求出 n，然后再遍历得到 n - k + 1

那么，我们能不能只遍历一次链表，就算出倒数第 k 个节点？假设 k = 2，思路如下：

1. 让指针 p1 指向 head，走 k 步
2. 现在的 p1，只要再走 n - k 步，就能走到链表末尾的空指针。此时，再用一个指针 p2 指向链表头节点 head；让 p1 和 p2 同时向前走，p1 走到链表末尾的空指针时前进了 n - k 步，p2 也从 head 开始前进了 n - k 步，停留在第 n - k + 1 个节点上，即恰好停链表的倒数第 k 个节点上
3. 这样，只遍历了一次链表，就获得了倒数第 k 个节点 p2。
   
![删除链表的倒数第N个节点](./photos/单链表倒数第k个节点.png)

代码实现如下：

```python
# 寻找链表的倒数第k个节点
def findLastK(self, head: Optional[ListNode], k) -> Optional[ListNode]:
  # p1先走k步
   p1 = head 
   for i in range(0, k):
       p1 = p1.next
   # 这个时候p2从head出发
   p2 = head
   # p2和p1都走n-k步（因为未知完整长度n，写代码不能写n），p1走到none
   while p1 is not None:
      p2 = p2.next
      p1 = p1.next
    # 此时p2的位置就是倒数第k个节点
   return p2
```

# Code

## ACM version

```python
import sys 

class ListNode:
    def __init__(self, val = 0, next = None):
        self.val = val
        self.next = next

class Solution:
    # 首先寻找倒数第k个节点-正数第n-k+1个节点
    def find(self, head, k):
        p1 = head
        # 快指针先走k步，此时指向第k+1个节点
        for _ in range(k):
            p1 = p1.next
        p2 = head # p2从第一个节点出发，还要走n-k步
        while p1:
            p1 = p1.next
            p2 = p2.next
        return p2
    
    # 小心要删除的节点是头结点，那么pre为None，所以要设置dummy处理边界
    def removeNthFromEnd(self, head, n):
        dummy = ListNode(-1)
        dummy.next = head # 此时dummy才是真正的“头结点”
        pre = self.find(dummy, n + 1) # 要找到倒数第n+1个节点（和加上dummy无关，dummy只是一个“头”）
        pre.next = pre.next.next
        return dummy.next


def build_list(array):
    dummy = ListNode(-1)
    p = dummy
    for num in array:
        p.next = ListNode(num)
        p = p.next
    return dummy.next


def print_list(head):
    vals = []
    p = head
    while p:
        vals.append(str(p.val))
        p = p.next
    print(len(vals), " ".join(vals))


def main():
    data = sys.stdin.read().strip().split("\n")
    idx = 0
    while idx < len(data):
        parts = list(map(int, data[idx].split()))
        length = parts[0]
        arr = list(map(int, parts[1: 1 + length])); idx += 1
        
        n = int(data[idx]); idx += 1
        
        head = build_list(arr)
        ans = Solution().removeNthFromEnd(head, n)
        print_list(ans)

if __name__ == "__main__":
    main()
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(n)

# 拓展
寻找单链表的中点：每当慢指针 slow 前进一步，快指针 fast 就前进两步，这样，当 fast 走到链表末尾时，slow 就指向了链表中点

```python
# 1->2->3->4->5，fast和slow都从head开始；终止条件是fast走到5，也就是fast.next=None
class Solution:
    # 快慢指针初始化指向 head
    def middleNode(self, head: ListNode) -> ListNode:
        slow = head
        fast = head
        # 快指针走到末尾时停止
        while fast and fast.next:
            # 慢指针走一步，快指针走两步
            slow = slow.next
            fast = fast.next.next
        # 慢指针指向中点
        return slow
```
