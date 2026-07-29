# Problem
https://labuladong.online/zh/problem/leetcode/reverse-nodes-in-k-group/description/


# Problem Description
给你链表的头节点 head，每 k 个节点一组进行翻转，请你返回修改后的链表。

k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

**你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。**


# Solution
1. 先翻转以 head 开头的 k 个节点
2. 将第 k + 1 个元素作为 head 递归调用 reverseKGroup 函数
3. 将上述两个过程的结果连接起来


# Code

## LC version

```python
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        # 特殊情况
        if head is None:
            return None
            
        # 仍然是左闭右开区间[a,b)包含k个要反转的元素
        a = b = head
        for i in range(k):
            # 不足k个，不需要翻转
            if b is None:
                return head
            b = b.next

        # 翻转前k个元素
        newHead = self.reverse(a, b)
        # 后面的也翻转
        a.next = self.reverseKGroup(b, k)
        return newHead

    
    # 反转区间 [a, b) 的元素，注意是左闭右开
    def reverse(self, a, b):
        pre = None
        cur = a
        nxt = a # pre -> cur -> nxt
        while cur != b:
            nxt = cur.next
            cur.next = pre
            pre = cur
            cur = nxt
        return pre
```

# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(1)
