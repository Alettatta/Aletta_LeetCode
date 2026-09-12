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
# 因为子数组长度不定，所以不用滑动窗口，用前缀和即可
# 子数组的sum如何用前缀和表示，因为前缀和到有效数组之前还存在一个gap，我们称为need
# need + k = preSum[i]，所以需要建立need的哈希表，如果确实存在need，就一定有这样的子数组

import sys 

class Solution:
    def subarraySum(self, nums, k):
        # 首先建立前缀和数组
        n = len(nums)
        preSum = [0] * (n + 1)
        preSum[0] = 0 # 实际需要计算1……n上面preSum的值，那么range到n+1

        # need，前缀和:该前缀和出现的次数
        count = {0: 1}
        ans = 0

        for i in range(1, n + 1):
            # 建立前缀和数组
            preSum[i] = preSum[i - 1] + nums[i - 1]
            # 处理gap问题
            need = preSum[i] - k
            if need in count:
                ans += count[need]
            # 这个gap不是我们需要的，但是要update一下count
            if preSum[i] not in count:
                count[preSum[i]] = 1
            else:
                count[preSum[i]] += 1
        return ans

data = sys.stdin.read().split() # 读取全部输入，全都split开，data是一个列表（里面都是字符串）
idx = 0
while idx < len(data):
    n = int(data[idx]); idx += 1
    k = int(data[idx]); idx += 1
    nums = [int(x) for x in data[idx: idx + n]]; idx += n
    print(Solution().subarraySum(nums, k))
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(n)
