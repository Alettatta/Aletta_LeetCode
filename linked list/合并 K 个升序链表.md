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
采用**分治思想**：把一个问题分解成若干个子问题，然后分别解决这些子问题，最后合并子问题的解得到原问题的解，这种思想广泛存在于递归算法中。

比如斐波那契数列的递归解法，把原问题 fib(n) 分解成 fib(n-1) 和 fib(n-2) 两个子问题，根据子问题的解合并得到原问题的解：
```c
int fib(int n) {
    // base case
    if (n == 0 || n == 1) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}
```

比如求一个数组的和：
```c
int getSum2(int[] nums, int start) {
    // base case
    if (start == nums.length) {
        return 0;
    }
    // nums[start..] 的元素和可以分解成第一个元素和剩余元素的和
    return nums[start] + getSum2(nums, start + 1);
}
```
递归调用需要 O(n) 的堆栈空间，所以空间复杂度是 O(n)；

时间复杂度等于递归调用的次数 x 每次递归调用的时间复杂度，递归调用的次数是 n，每次递归调用只做一次加法操作，时间复杂度是 O(1)，所以总的时间复杂度是 O(n)。

把数组分成两半，分别求和，最后把两半的和相加：

```c
int getSum3(int[] nums, int start, int end) {
    // base case
    if (start == end) {
        return nums[start];
    }

    int mid = start + (end - start) / 2;
    // 计算 nums[start..mid] 的和
    int leftSum = getSum3(nums, start, mid);
    // 计算 nums[mid+1..end] 的和
    int rightSum = getSum3(nums, mid + 1, end);

    // 合并得到 nums[start..end] 的和
    return leftSum + rightSum;
}
```
getSum3 算法从中间二分，递归树就是一个较为平衡的二叉树，所以堆栈（树高）的空间复杂度是 O(logn)。


# Code

## LC version

```python
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        # 采用分治思想，首先考虑合并2个升序链表，然后使用递归
        # 因为遍历整个数组来不断进行两两合并很耗费时间复杂度，可以从中间二分
        if len(lists) == 0:
            return None # return []因为要求返回要么listnode要么none
        return self.mergeKLists2(lists, 0, len(lists) - 1)

    # 定义：合并 lists[start..end] 为一个有序链表
    def mergeKLists2(self, lists: list['ListNode'], start: int, end: int) -> Optional[ListNode]:
        if start == end: # 只有一组元素
            return lists[start]
        mid = (start + end) // 2
        left = self.mergeKLists2(lists, start, mid) # 合并左半边 lists[start..mid] 为一个有序链表
        right = self.mergeKLists2(lists, mid + 1, end) # 合并右半边 lists[mid+1..end] 为一个有序链表
        return self.mergeTwoLists(left, right)
        
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(-1)
        p = dummy
        p1, p2 = list1, list2

        while p1 is not None and p2 is not None:
            if p1.val < p2.val:
                p.next = p1
                p1 = p1.next
            else:
                p.next = p2
                p2 = p2.next
            p = p.next

        if p1 is not None:
            p.next = p1
        if p2 is not None:
            p.next = p2

        return dummy.next
```

# Complexity Analysis
- 该算法的时间复杂度相当于是把 k 条链表分别遍历 O(logk) 次。那么假设 k 条链表的元素总数是 N，该算法的时间复杂度就是 O(Nlogk)
- 该算法的空间复杂度只有递归树堆栈的开销，也就是 O(logk)
