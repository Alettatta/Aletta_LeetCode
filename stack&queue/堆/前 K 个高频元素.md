# Problem
https://labuladong.online/zh/problem/leetcode/top-k-frequent-elements/description/


# Problem Description
给你一个整数数组 nums 和一个整数 k，请你返回其中出现频率前 k 高的元素。

输出要求：

为保证答案唯一，请将返回的 k 个元素按出现频率从高到低排序；当出现频率相同时，按元素值从小到大排序。

给你一个整数数组 nums 和一个整数 k，请返回数组中出现频率前 k 高的元素，按频率降序排列；频率相同时按元素值升序排列。



# Code

## LC version

首先用一个哈希表 freq 统计出每个元素出现的频率，这样原问题就转化为「按频率挑出前 k 个元素」。

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        freq = Counter(nums)
        unique = sorted(freq.keys(), key = lambda x:(-freq[x], x))
        return unique[:k]
```

Counter统计每个元素出现了多少次，类似{1: 3, 2: 2, 3: 1}

因此freq.keys就是键值（原本的nums元素）

对于每个数字 x，排序依据是一个元组：(-freq[x], x)。其中：-freq[x]：按照出现次数从高到低排序；x：频率相同时，按照数字从小到大排序。


## ACM version

**ACM 模式的注意点：**

- 需要 import 完整的类（包括 sys、typing 等）
- 数据在标准输入流 stdin 中，全部是原始的文本字符串
- 必须用 print() 手动将结果写到标准输出流 stdout
- 需要写 while 或 for line in sys.stdin 循环处理，直到文件结束（EOF）

```python
import sys
from collections import Counter
from typing import List

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        freq = Counter(nums)
        unique = sorted(freq.keys(), key = lambda x:(-freq[x], x))
        return unique[:k]

data = sys.stdin.read().split()
idx = 0
while idx < len(data):
    n = int(data[idx])
    k = int(data[idx + 1])
    idx += 2
    nums = [int(x) for x in data[idx:idx + n]]
    idx += n
    result = Solution().topKFrequent(nums, k)
    print(" ".join(map(str, result)))
```


# Complexity Analysis
- 时间复杂度：O(n+dlogd)，其中 n 是数组长度，d 是不同元素的个数。统计频率是 O(n)，对 d 个不同元素排序是 O(dlogd)。
- 空间复杂度：O(d)，哈希表和排序用的数组都只存不同的元素。
