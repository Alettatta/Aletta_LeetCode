# Problem
https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/submissions/734416073/


# Problem Description
给你一个下标从 1 开始的整数数组 numbers ，该数组已按 非递减顺序排列  ，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果设这两个数分别是 numbers[index1] 和 numbers[index2] ，则 1 <= index1 < index2 <= numbers.length 。

以长度为 2 的整数数组 [index1, index2] 的形式返回这两个整数的下标 index1 和 index2。

你可以假设每个输入 只对应唯一的答案 ，而且你 不可以 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。



# Key Points
与 hot100[1]两数之和 不一样的是，本题的 nums 已经有序，可以使用左右指针解决（hot100[1]用不了是因为 nums.sort() 就改变了索引）


# Code
```python
# 如果使用双指针的做法
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        left, right = 0, len(nums) - 1
        while left < right:
            if nums[left] + nums[right] < target:
                left += 1
            elif nums[left] + nums[right] > target:
                right -= 1
            else:
                return [left+1, right+1]
        return []
```




# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(1)

# Further Thinking
如果我们不需要返回索引，而是返回数字，例如 nums = [1,1,1,2,2,3,3], target = 4，得到的结果中 [1,3] 肯定会重复。

出问题的地方在于 sum == target 条件的 if 分支，当给 ans 加入一次结果后，left 和 right 不仅应该相向而行，而且应该跳过所有重复的元素，那么我们可以得到优化 if 条件的代码：

```python
def twoSumTargert(nums, target):
    # nums 数组必须有序
    nums.sort()
    lo, hi = 0, len(nums) - 1
    res = []
    while lo < hi:
        sum = nums[lo] + nums[hi]
        left, right = nums[lo], nums[hi]
        if sum < target:
            while lo < hi and nums[lo] == left: # 注意nums[lo] == left，需要跳过这个数字
                lo += 1
        elif sum > target:
            while lo < hi and nums[hi] == right: # 同理
                hi -= 1
        else:
            res.append([left, right])
            while lo < hi and nums[lo] == left: # 同理
                lo += 1
            while lo < hi and nums[hi] == right: # 同理
                hi -= 1
    return res
```
    
