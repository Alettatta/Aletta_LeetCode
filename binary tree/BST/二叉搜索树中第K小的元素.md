# Problem
https://labuladong.online/zh/problem/leetcode/kth-smallest-element-in-a-bst/description/


# Problem Description

给定一个二叉搜索树（BST）的根节点 root，以及一个整数 k，请你设计一个算法查找其中第 k 小的元素（从 1 开始计数）。


# Solution
利用BST性质，实际考察中序遍历


# Code

## LC version

```python
class Solution:
    # 中序遍历二叉搜索树，可以得到一个有序数组，然后查第k小的数字（k>=1）
    def __init__(self):
        self.rank = 0 # 当前元素的排名
        self.ans = 0
    
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        self.traverse(root, k)
        return self.ans

    def traverse(self, root, k):
        if root is None:
            return
            
        self.traverse(root.left, k)

        self.rank += 1
        if k == self.rank:
            self.ans = root.val
            return
        
        self.traverse(root.right, k)
```

# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(1)
