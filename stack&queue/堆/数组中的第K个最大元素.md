# Problem
https://labuladong.online/zh/problem/leetcode/kth-largest-element-in-an-array/description/


# Problem Description

给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。即，将数组降序排序后，排在第 k 位的元素。

你可以假设 k 总是有效的，且 1 <= k <= nums.length。


# Solution

可以把**小顶堆（每个节点下方的所有节点的值都比它大）** pq 理解成一个筛子，较大的元素会沉淀下去，较小的元素会浮上来；当堆大小超过 k 的时候，我们就删掉堆顶的元素，因为这些元素比较小，而我们想要的是前 k 个最大元素嘛。

当 nums 中的所有元素都过了一遍之后，筛子里面留下的就是最大的 k 个元素，而堆顶元素是堆中最小的元素，也就是「第 k 个最大的元素」。

二叉堆插入和删除的时间复杂度和堆中的元素个数有关，在这里我们堆的大小不会超过 k，所以插入和删除元素的复杂度是 O(logK)，再套一层 for 循环，总的时间复杂度就是 O(NlogK)。


# Code

## LC version

```python
import heapq

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        pq = [] # 建立小顶堆，堆顶是最小元素
        for e in nums:
            heapq.heappush(pq, e) # 每个元素都要过一遍二叉堆
            if len(pq) > k:
                heapq.heappop(pq) # 堆中元素多于 k 个时，删除堆顶元素
        return pq[0] # pq 中剩下的是 nums 中 k 个最大元素，堆顶是最小的那个，即第 k 个最大元素
```

## ACM version

**ACM 模式的注意点：**

- 需要 import 完整的类（包括 sys、typing 等）
- 数据在标准输入流 stdin 中，全部是原始的文本字符串
- 必须用 print() 手动将结果写到标准输出流 stdout
- 需要写 while 或 for line in sys.stdin 循环处理，直到文件结束（EOF）

```python
import sys
from typing import List
import heapq


class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        pq = [] # 建立小顶堆，堆顶是最小元素
        for e in nums:
            heapq.heappush(pq, e) # 每个元素都要过一遍二叉堆
            if len(pq) > k:
                heapq.heappop(pq) # 堆中元素多于 k 个时，删除堆顶元素
        return pq[0] # pq 中剩下的是 nums 中 k 个最大元素，堆顶是最小的那个，即第 k 个最大元素


header = None
for line in sys.stdin:
    parts = line.strip().split()
    if not parts:
        continue
    if header is None:
        header = (int(parts[0]), int(parts[1]))
    else:
        n, k = header
        nums = list(map(int, parts))
        result = Solution().findKthLargest(nums, k)
        print(result)

        header = None
```


# Complexity Analysis
- 时间复杂度：O(nlogn)
- 空间复杂度：O(n)
