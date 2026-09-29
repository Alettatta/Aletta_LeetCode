# Problem
https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/description/


# Problem Description

给定一个 已排序 的链表的头节点 head。

删除原始链表中所有 重复 数字的节点，只留下 不同 的数字。

返回 已排序 的链表。


# Solution
所有删除问题都考虑头结点被删除的情况，所以要引入dummy

那么指针就从dummy开始，如果有重复元素就全部next跳过

PS.不用考虑指针手动置空next，因为一旦没有任何指针指向某个节点，它就被垃圾回收了

# Code

## LC version

```python
class Solution:
    def deleteDuplicates(self, head: ListNode | None) -> ListNode | None:
        # 万一头结点被删除，需要dummy
        dummy = ListNode(-1)
        p = dummy
        p.next = head
        while p.next and p.next.next:
            if p.next.val == p.next.next.val:
                val = p.next.val # 提取这个元素
                while p.next and p.next.val == val: # 跳过所有这个元素
                    p.next = p.next.next
            else:
                p = p.next
        return dummy.next
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(1)
