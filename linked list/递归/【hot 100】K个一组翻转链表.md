# Problem
https://labuladong.online/zh/problem/leetcode/reverse-nodes-in-k-group/description/


# Problem Description
给你链表的头节点 head，每 k 个节点一组进行翻转，请你返回修改后的链表。

k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。


# Solution
1. 先翻转以 head 开头的 k 个节点
2. 将第 k + 1 个元素作为 head 递归调用 reverseKGroup 函数
3. 将上述两个过程的结果连接起来

**注意，只需要划分为2个过程，而非k个过程；原因在于step 2的递归调用**

# Code

## LC version

```python
# 反转head开头的k个节点->移到k+1位置(即successor)处继续调用reverseKGroup->这两段拼接起来

class Solution:
    def __init__(self):
        self.successor = None
        
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        # 看个数够不够k
        node = head
        for _ in range(k):
            if not node:
                return head
            node = node.next
        
        first = self.reverseN(head, k) # 反转head开头的k个节点
        head.next = self.reverseKGroup(self.successor, k) # 从k+1位置开始再每k个反转
        return first
        
    def reverseN(self, head, n): # 反转以head开头的前N个节点
        if n == 1:
            self.successor = head.next
            return head
        second = self.reverseN(head.next, n - 1)
        head.next.next = head
        head.next = self.successor
        return second
```

# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(1)
