# Problem
https://labuladong.online/zh/problem/leetcode/two-sum/description/


# Problem Description
给你一个整数数组 nums 和一个整数 target，请你在该数组中找出 和为目标值 target 的两个整数，并返回它们的数组下标。
你可以假设每种输入 只会对应一个答案，并且不能使用数组中同一个元素两次。

输出要求：
返回的两个下标必须按 升序 排列（即较小的下标在前），以保证答案唯一。

数据范围：
2 <= nums.length <= 10^4
-10^9 <= nums[i] <= 10^9
-10^9 <= target <= 10^9
只会存在一个有效答案


# Key Points
需要遍历全部的元素，想哈希表方法（快速判断一个元素是否出现过）

哈希存储的数据结构：
- 数组（元素数量有限，例如字母a-z）
- set（元素数量未定，注意集合的元素不能重复）
- dict（map，注意确定好谁是key谁是value）


# Solution
使用 dict 可以同时存储 index 和 value


# Code

## LC version

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 要遍历每一个元素，需要存储元素的值和下标——使用哈希存储
        hash = dict()
        for index, num in enumerate(nums):
            if target - num in hash:
                return [index, hash[target - num]]
            hash[num] = index
        return
```

## ACM version

**ACM 模式的注意点：**

- 需要 import 完整的类（包括 sys、typing 等）
- 数据在标准输入流 stdin 中，全部是原始的文本字符串
- 必须用 print() 手动将结果写到标准输出流 stdout
- 需要写 while 或 for line in sys.stdin 循环处理，直到文件结束（EOF）

```python
import sys # 引入系统模块，读取标准输入
from typing import List # 引入List类型

# 核心代码（LC部分）
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hash = dict()
        for index, num in enumerate(nums):
            if target - num in hash:
                return[index, hash[target - num]]
            hash[num] = index
        return 

# ACM读取输入输出部分，支持多组示例
for line in sys.stdin: # 每次读取一行输入
    parts = line.strip().split() # 第一行：去掉首尾空白，按空格切分
    if len(parts) < 2: # 题目要求第一行表示数组长度和目标值，有两个数
        continue # 所以如果有一行不足2个数，跳过
    n = int(parts[0]) # 输入数组长度
    target = int(parts[1]) # 输入目标值
    
    nums_line = sys.stdin.readline() # 再读新的一行
    nums = list(map(int, nums_line.strip().split())) # nums_line去掉首尾空白，按空格切分，转化为整数，变成列表
    result = Solution().twoSum(nums, target) # 计算结果
    if len(result) == 2 and result[0] > result[1]:
        result[0], result[1] = result[1], result[0] # 要求下标升序

    print(" ".join(str(x) for x in result)) # 输出两个下标，空格分隔（看ACM要求输出）
```


# Complexity Analysis

因为最长需要遍历整个数组，dict 最长也存储整个数组。

- 时间复杂度：O(n)
- 空间复杂度：O(n)
