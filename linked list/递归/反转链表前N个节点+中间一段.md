# 补充问题：反转链表的前N个节点

![反转链表前N个节点](../photos/反转链表前N个节点.jpg)

比如1-2-3-4-5-6，反转前3个节点得到3-2-1-4-5-6

**依然采用递归的思想：需要反转前N个节点，也就是反转前N-1个节点、第N个节点指向head，然后head.next=N+1个节点**

```python
class Solution:
    def __init__(self):# 记住“没被反转的那部分的头”，让反转后的尾巴能接上它
        self.successor = None # 记录第N+1个节点

    # 递归结构：反转前N个节点也就是反转前N-1个节点，然后第N个节点指向head
    def reverseN(self, head, n):
        if n == 1:
            self.successor = head.next
            return head
        new_head = self.reverseN(head, n - 1)
        head.next.next = head # 3.next = 1
        head.next = self.successor
        return new_head
```

# Problem
https://leetcode.cn/problems/reverse-linked-list-ii/description/


# Problem Description

给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表 。



# Code

## LC version

```python
# 递归：反转从left开始的right-left+1个节点，复用reverseN即可，问题在于如何找到left？
class Solution:
    def __init__(self):
        self.successor = None

    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
        dummy = ListNode(-1) # 处理left-1情况
        dummy.next = head
        p = dummy
        for _ in range(left - 1): # p为left-1节点
            p = p.next
        second = self.reverseN(p.next, right - left + 1)
        p.next = second
        return dummy.next
    
    # 反转从head开始的n个节点
    def reverseN(self, head, n):
        if n == 1:
            self.successor = head.next
            return head
        new_head = self.reverseN(head.next, n -1)
        head.next.next = head
        head.next = self.successor
        return new_head
```
