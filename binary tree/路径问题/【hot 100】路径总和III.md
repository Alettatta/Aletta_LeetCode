# Problem
https://labuladong.online/zh/problem/leetcode/path-sum-iii/description/


# Problem Description
给定一个二叉树的根节点 root，和一个整数 targetSum，求该二叉树里节点值之和等于 targetSum 的路径的数目。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

# Solution
递归思想：所有路径之和=root开始的自己的路径+root.left所有路径+root.right所有路径

所以主函数是所有路径，另外加一个函数记录单个node出发的路径有几条


# Code

## LC version

```python
class Solution:
    def pathSum(self, root, targetSum): # 计算整棵树的路径和=所有节点路径总和=root+root.left+root.right
        if root is None:
            return 0
        return (self.singleCount(root, targetSum) # root本身开始的路径和
                + self.pathSum(root.left, targetSum) # 左子树所有路径和
                + self.pathSum(root.right, targetSum)) # 右子树左右路径和

    def singleCount(self, node, targetSum): # 计算从单个节点node开始的路径有几条
        if node is None:
            return 0
        cnt = 0
        if node.val == targetSum:
            cnt = 1
        cnt += self.singleCount(node.left, targetSum - node.val) # 从node开始找，所以先去掉node
        cnt += self.singleCount(node.right, targetSum - node.val)
        return cnt
```


# Complexity Analysis
- 时间复杂度：O(n^2)
- 空间复杂度：O(n)
