# Problem

https://leetcode.cn/problems/add-two-numbers-ii/description/

# Problem Description

给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。


# Solution
从低位开始相加更简单，所以先反转链表，代码如下

```python
def reverse(head):
  pre = None
  while head:
    nxt = head.next
    head.next = pre
    pre = head
    head = nxt
  return pre
```


# Code

## ACM version


```python
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        # 因为从低位开始加比较符合数学运算逻辑，所以先反转链表
        list1 = self.reverse(l1) # 3->4->2->7
        list2 = self.reverse(l2) # 4->6->5

        dummy = ListNode(-1) # 因为需要重建链表所以设置dummy
        p1, p2 = list1, list2
        p = dummy
        carry = 0

        while p1 is not None or p2 is not None or carry > 0:
            v1 = p1.val if p1 is not None else 0
            v2 = p2.val if p2 is not None else 0

            total = v1 + v2 + carry
            carry = total // 10 # 进位
            p.next = ListNode(total % 10) # 当前位
            p = p.next
            
            # 把更长的位数放在total中
            if p1 is not None:
                p1 = p1.next
            if p2 is not None:
                p2 = p2.next

        return self.reverse(dummy.next) # 注意结果也需要反转回去，保证高位在前面


    def reverse(self, head):
        pre = None
        while head is not None:
            nxt = head.next
            head.next = pre # 其实重点就是这句，重置pre和head的关系
            pre = head
            head = nxt
        return pre
# None -> 1 -> 2 -> 3 -> 4
# head = 1, nxt = 2
# 2 -> 1 -> None; pre = 1, head = 2
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(1)


