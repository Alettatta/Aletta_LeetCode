# Problem
https://labuladong.online/zh/problem/leetcode/linked-list-cycle-ii/description/


# Problem Description
给定一个链表的头节点 head，返回链表开始入环的第一个节点。如果链表无环，则返回 null。

如果链表中有某个节点，可以通过持续追踪 next 指针再次到达，则链表中存在环。系统内部使用整数 pos 表示链表尾部连接到的节点下标（下标从 0 开始）。如果 pos 为 -1，则说明该链表中没有环。注意：pos 仅用于标识环的位置，并不会作为参数传递到函数中。

不允许修改链表。


# Key Points
如果单纯判断链表有没有环，参考代码：

```python
class Solution:
    # 快慢指针初始化指向 head
    def hasCycle(self, head: ListNode) -> bool:
        slow = head
        fast = head
        # 快指针走到末尾时停止
        while fast is not None and fast.next is not None:
            # 慢指针走一步，快指针走两步
            slow = slow.next
            fast = fast.next.next
            # 快慢指针相遇，说明含有环
            if slow == fast:
                return True
        # 不包含环
        return False
```

现在需要寻找环的起点，和[寻找重复数]一样，思路如下：

1. 还是快慢指针，fast++2，slow++1
2. 快慢指针相遇，此时fast走了2k步，慢指针走了k步，从图中可以看出，fast多走的k步是环长的倍数
![环形链表II](./photos/环形链表1-副本.jpeg)
3. 此时我们引入环的入口，假设相遇点距离环的入口是m，那么结合上图的 slow 指针，环的起点距头结点 head 的距离为 k - m，也就是说如果从 head 前进 k - m 步就能到达环起点。
4. 巧的是，如果从相遇点继续前进 k - m 步，也恰好到达环起点。因为结合上图的 fast 指针，从相遇点开始走k步可以转回到相遇点，那走 k - m 步肯定就走到环起点了：
![环形链表II](./photos/环形链表2-副本.jpeg)
5. 因此，可以把快慢指针中的一个移到head，两个指针都++1前进，k-m步后便会在环的入口相遇

# Code

## LC version

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast = head
        slow = head
        while fast is not None and fast.next is not None:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                break # 找到相遇点

        # 排除没有环的情况：
        if fast == None or fast.next == None:
            return None
            
        slow = head # slow移到head，重新走k-m
        while fast != slow: # 注意这个条件，因为上面已经排除了没有环的情况，肯定不是none
            fast = fast.next
            slow = slow.next
        return slow
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(1)
