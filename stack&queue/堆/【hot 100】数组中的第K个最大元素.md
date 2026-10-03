# 补充：数据流中的第K大元素
https://leetcode.cn/problems/kth-largest-element-in-a-stream/

设计一个找到数据流中第 k 大元素的类（class）。注意是排序后的第 k 大元素，不是第 k 个不同的元素。

请实现 KthLargest 类：

KthLargest(int k, int[] nums) 使用整数 k 和整数流 nums 初始化对象。

int add(int val) 将 val 插入数据流 nums 后，返回当前数据流中第 k 大的元素。

```python
# 模拟一下k=3, nums = [4,5,8,2]情形
# add(3):[2,3,4,5,8]，第三大为4
# add(5):[2,3,4,5,5,8]，第三大为5
# add(10):[2,3,4,5,5,8,10]，第三大为5
# add(9):[2,3,4,5,5,8,9,10]，第三大为8
# add(4):[2,3,4,4,5,5,8,9,10]，第三大为8
# 求第K大元素：倒着看nums，提取K个元素，要求的数字为K个中的最小值，所以维护一个大小为K的小顶堆即可————堆中始终保存当前数据流中最大的 k 个元素；堆顶就是这 k 个元素中最小的那个，也就是第 k 大的元素

import heapq

class KthLargest:
    # 初始化，nums里面的元素依次加入堆，如果堆的大小超过K就弹出堆顶
    def __init__(self, k: int, nums: list[int]):
        self.k = k 
        self.heap = nums[:]
        heapq.heapify(self.heap) # 先整体堆化
        while len(self.heap) > k:
            heapq.heappop(self.heap) # 循环弹出，直到只剩 k 个

    # 把 val 加入堆；如果堆的大小超过 k，弹出堆顶；返回堆顶（第 k 大元素）
    def add(self, val: int) -> int:
        heapq.heappush(self.heap, val)
        if len(self.heap) > self.k:
            heapq.heappop(self.heap)
        return self.heap[0]
```

# Problem
https://labuladong.online/zh/problem/leetcode/kth-largest-element-in-an-array/description/


# Problem Description

给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。即，将数组降序排序后，排在第 k 位的元素。

你可以假设 k 总是有效的，且 1 <= k <= nums.length。


# Solution

注意：**heapify 是一次性把一个无序列表整理成堆，而 heappush 是每次插入时增量维护。两者选一个即可**


# Code


## ACM version


```python
import sys 
import heapq

# 找nums中第K大元素，维护一个大小为K的小顶堆，堆顶就是答案
class Solution:   
    def findKthLargest(self, nums, k):
        heap = []
        for num in nums:
            heapq.heappush(heap, num)
            if len(heap) > k:
                heapq.heappop(heap)
        return heap[0]

data = sys.stdin.read().strip().split('\n')
idx = 0
while idx < len(data):
    n, k = map(int, data[idx].split()); idx += 1
    nums = list(map(int, data[idx].split())); idx += 1
    ans = Solution().findKthLargest(nums, k)
    print(ans)
```


# Complexity Analysis
- 时间复杂度：O(nlogn)
- 空间复杂度：O(n)
