# Problem
https://labuladong.online/zh/problem/leetcode/kth-smallest-element-in-a-bst/description/


# Problem Description

给定一个二叉搜索树（BST）的根节点 root，以及一个整数 k，请你设计一个算法查找其中第 k 小的元素（从 1 开始计数）。


# Solution
利用BST性质，实际考察中序遍历


# Code

## ACM version

```python
import sys 
from collections import deque

class TreeNode:
    def __init__(self, val = 0, left = None, right = None):
        self.val = val
        self.left = left
        self.right = right

# 中序遍历BST，然后取索引为k-1的元素
class Solution:
    def kthSmallest(self, root, k):
        ans = []
        
        def traverse(root):
            if root is None:
                return
            traverse(root.left)
            ans.append(root.val)
            traverse(root.right)
        
        traverse(root)
        return ans[k - 1]

# input: '1 2 3 4 5' -> BinaryTree
def build_tree(s):
    data = s.strip().split()
    if not data:
        return None
    if data[0] == '#':
        return None

    root = TreeNode(int(data[0]))
    idx = 1
    q = deque([root])

    while q:
        cur = q.popleft()
        if idx < len(data):
            t = data[idx]; idx += 1
            if t != '#':
                cur.left = TreeNode(int(t))
                q.append(cur.left)
        if idx < len(data):
            t = data[idx]; idx += 1
            if t != '#':
                cur.right = TreeNode(int(t))
                q.append(cur.right)
    
    return root 

data = sys.stdin.read().strip().split('\n')
idx = 0
while idx < len(data):
    length = int(data[idx]); idx += 1
    s = data[idx] if idx < len(data) else ''; idx += 1
    root = build_tree(s)
    k = int(data[idx]); idx += 1
    result = Solution().kthSmallest(root, k)
    print(result)
```

# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(n)
