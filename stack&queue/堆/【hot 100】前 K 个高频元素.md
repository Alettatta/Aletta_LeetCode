# Problem
https://labuladong.online/zh/problem/leetcode/top-k-frequent-elements/description/


# Problem Description
给你一个整数数组 nums 和一个整数 k，请你返回其中出现频率前 k 高的元素。

输出要求：

为保证答案唯一，请将返回的 k 个元素按出现频率从高到低排序；当出现频率相同时，按元素值从小到大排序。

给你一个整数数组 nums 和一个整数 k，请返回数组中出现频率前 k 高的元素，按频率降序排列；频率相同时按元素值升序排列。

# Solution
用小顶堆就可以解决，因为没有要求输出要从大到小

如何在heap中push元组：heapq.heappush(heap, (freq, num))

如果两个元素频率相同，Python 会继续比较第二个元素 num。如果 num 不可比较（比如是对象），可以在中间加一个唯一序号避免比较：heapq.heappush(heap, (freq, i, num))。其中i是递增计数器。

# Code


## ACM version

```python
import heapq
from collections import defaultdict
import sys

class Solution:
    def topKFrequent(self, nums, k):
        # 维护一个大小为K的小顶堆，堆顶是使用频率最低的，依次往下越来越高
        # 维护的依据是nums中元素出现次数
        hash = defaultdict(int)
        for num in nums:
            hash[num] += 1 # {1:3,2:1,3:1}; num: count

        heap = []
        for num, freq in hash.items():
            heapq.heappush(heap, (freq, num)) # freq是第一比较依据，其次才是num
            if len(heap) > k:
                heapq.heappop(heap)

        return [num for freq, num in heap] # heap里面存储的是元组，但是我只要num，不要频率输出

data = sys.stdin.read().strip().split('\n')
idx = 0
while idx < len(data):
    n, k = map(int, data[idx].split()); idx += 1
    nums = list(map(int, data[idx].split())); idx += 1
    ans = Solution().topKFrequent(nums, k)
    print(' '.join(str(num) for num in ans))
```


# Complexity Analysis
- 时间复杂度：O(n+dlogd)，其中 n 是数组长度，d 是不同元素的个数。统计频率是 O(n)，对 d 个不同元素排序是 O(dlogd)。
- 空间复杂度：O(d)，哈希表和排序用的数组都只存不同的元素。
