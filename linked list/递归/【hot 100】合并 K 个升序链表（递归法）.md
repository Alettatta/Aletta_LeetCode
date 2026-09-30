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

# 补充：关于递归

**考虑问题能否用递归解决：这个问题是否存在子问题结构？**

**采用分治思想：通过递归函数的定义，把原问题分解成若干规模更小、结构相同的子问题，最后通过子问题的答案组装原问题的解。**

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
# 逐一合并，先合并2个链表，然后结果继续往下合并
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        if not lists:
            return None

        ans = None
        for node in lists:
            ans = self.mergeTwoLists(ans, node)
        return ans

    def mergeTwoLists(self, l1, l2):
        dummy = ListNode(-1)
        p = dummy
        while l1 and l2:
            if l1.val <= l2.val:
                p.next = l1
                l1 = l1.next
            else:
                p.next = l2
                l2 = l2.next
            p = p.next
        p.next = l1 if l1 else l2
        return dummy.next
```

# Complexity Analysis
- 该算法的时间复杂度相当于是把 k 条链表分别遍历 O(logk) 次。那么假设 k 条链表的元素总数是 N，该算法的时间复杂度就是 O(Nlogk)
- 该算法的空间复杂度只有递归树堆栈的开销，也就是 O(logk)
