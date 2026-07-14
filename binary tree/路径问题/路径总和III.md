# Problem
https://labuladong.online/zh/problem/leetcode/path-sum-iii/description/


# Problem Description
给定一个二叉树的根节点 root，和一个整数 targetSum，求该二叉树里节点值之和等于 targetSum 的路径的数目。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。


# Solution
思路使用枚举，每个节点要干什么？这个“干什么”放在前中后序哪里？

干什么（核心就是need = preSum - targetSum）：

1. 算出"从根到我自己"的路径和 preSum 
2. 回答"以我结尾、和为targetSum的路径,有几条?"**只要在"我的所有祖先"里,存在一个祖先的preSum恰好等于 need,那么"那个祖先到我"就是一条合法路径。有几个这样的祖先,就有几条路径。**
3. 把自己的preSum"登记"进公共记录里,方便自己的子孙节点查询
4. 处理完自己之后,让自己的左右子树也各自完成这四件事
5. (容易被忽略,但极其重要):当我自己和我的整个子树都处理完了,要"退出"了,必须把第三件事登记的东西撤销掉，不妨碍其他堂兄弟

![路径总和](./photos/路径总和III)

放在哪里：根据上面的顺序，是前序

# Code

## LC version

```python
class Solution:
    # 从root到叶子结点所有路中，像大小不定的滑动窗口看是否== targetSum，需要用前缀和
    # 采用枚举的思路，所以要traverse所有节点
    
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        self.targetSum = targetSum # 全局变量，下面也可以用
        
        self.ans = 0
        self.count = {0:1} # key：前缀和，value: 该前缀和出现的次数
        # 要加上{0:1}：这棵树只有一个节点,值为5,正好等于 targetSum。这条"只包含根节点"的路径,应该被算作1条有效路径。
        self.traverse(root, 0) # 需要遍历所有节点, 传入初始preSum=0
        return self.ans

    def traverse(self, root, preSum):
        if root is None:
            return
            
        preSum = preSum + root.val # 当前这条路径（根到root）的前缀和
        
        # 如果存在值为need的前缀和，说明存在一条以root结尾、和为targetSum的路径
        need = preSum - self.targetSum

        if need in self.count:
            self.ans += self.count[need]

        if preSum not in self.count:
            self.count[preSum] = 1
        else:
            self.count[preSum] += 1

        self.traverse(root.left, preSum) # 当前值传给子节点再用
        self.traverse(root.right, preSum)

        self.count[preSum] -= 1 # 回溯：撤销当前节点对count的影响，避免影响其他分支（比如兄弟子树）

        return root
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(n)
