# Aletta_LeetCode
Aletta's LeetCode Learning Journey……

## 数据结构与算法总纲
![labuladong](./photos/labuladong.png)

**数据结构**的关键点在于遍历和访问，即**增删查改**等基本操作。种种数据结构，皆为**数组**（顺序存储）和**链表**（链式存储）的变换。

种种**算法**，皆为**穷举**。穷举的关键点在于**无遗漏和无冗余**。熟练掌握算法框架，可以做到无遗漏；充分利用信息，可以做到无冗余。


## ACM模式要点

- 需要 import 完整的类（包括 sys、typing 等）
- 数据在标准输入流 stdin 中，全部是原始的文本字符串
- 需要写 while 或 for line in sys.stdin 循环处理，直到文件结束（EOF）
- 必须用 print() 手动将结果写到标准输出流 stdout
- PS: line和readline的区别，line每行都一样；readline有行和其他不一样，比如我需要抛开第一行循环读接下来的；每调用一次 readline()，指针就自动往下走一行

```python
# 给你输入两个整数 a 和 b，请你计算它们的和

import sys 

class Solution():
    def add(self, a: int, b: int) -> int:
        return a + b

for line in sys.stdin: # sys.stdin是sys的标准输入流，和input()相比可以输入大量数据、快速
    a, b = map(int, line.strip().split())
    # line末尾带有换行符"\n"，strip去掉首末尾所有空白，比如"3 5 \n"变成"3 5"
    # split按照空白字符把字符串切分成列表['3','5']
    # map(函数, 可迭代对象) 把函数依次作用到可迭代对象的每个元素上，map(int, ["3", "5"])把每个字符串转成整数
    result = Solution().add(a, b)
    print(result)
```

```python
# 请你计算一个长度为 n 的一维数组 nums 中所有元素之和；但是有 T 组数组。

import sys
from typing import List # typing 里的类型注解，因为有List[int]所以需要import

class Solution():
    def sumArray(self, nums: List[int]) -> int:
        ans = 0
        for num in nums:
            ans += num
        return ans

for line in sys.stdin:
    t = int(line.strip()) # 数组个数T
    for _ in range(t): # 一共T组，T个遍历
        n = int(sys.stdin.readline().strip()) # 当前数组长度
        nums = list(map(int, sys.stdin.readline().strip().split()))
        print(Solution().sumArray(nums))
```
