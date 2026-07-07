# Problem
https://labuladong.online/zh/problem/leetcode/subarray-sum-equals-k/description/


# Problem Description
给你一个整数数组 nums 和一个整数 k，请你统计并返回该数组中和为 k 的子数组的个数。

子数组 是数组中元素的连续非空序列。


# Key Points
不是滑动窗口而是前缀和



# Code

## LC version

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        # 第一想法是滑动窗口，但是不知道数组中元素的大小关系（无序），无法判断要不要扩大/缩小窗口
        n = len(nums)
        preSum = [0] * (n+1)
        preSum[0] = 0
        ans = 0
        count = {0:1} # 前缀和到该前缀和出现次数的映射
        
        for i in range(1, n+1):
            preSum[i] = preSum[i-1] + nums[i-1]
            need = preSum[i] - k # 如果存在值为need的前缀和，说明存在以nums[i-1]结尾的子数组和为k
            if need in count:
                ans += count[need]
            if preSum[i] not in count:
                count[preSum[i]] = 1
            else:
                count[preSum[i]] += 1
                
        return ans
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

class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        # 第一想法是滑动窗口，但是不知道数组中元素的大小关系（无序），无法判断要不要扩大/缩小窗口
        n = len(nums)
        preSum = [0] * (n+1)
        preSum[0] = 0
        ans = 0
        count = {0:1} # 前缀和到该前缀和出现次数的映射
        
        for i in range(1, n+1):
            preSum[i] = preSum[i-1] + nums[i-1]
            need = preSum[i] - k # 如果存在值为need的前缀和，说明存在以nums[i-1]结尾的子数组和为k
            if need in count:
                ans += count[need]
            if preSum[i] not in count:
                count[preSum[i]] = 1
            else:
                count[preSum[i]] += 1
                
        return ans

tokens = []
for line in sys.stdin:
    tokens.extend(line.split())

idx = 0
while idx < len(tokens):
    n = int(tokens[idx]); idx += 1
    k = int(tokens[idx]); idx += 1
    nums = [int(tokens[idx + i]) for i in range(n)]
    idx += n
    result = Solution().subarraySum(nums, k)

    print(result)
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(n)
