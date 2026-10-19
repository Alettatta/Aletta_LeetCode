# Problem
https://labuladong.online/zh/problem/leetcode/implement-trie-prefix-tree/description/


# Problem Description
Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。

请你实现 Trie 类：

Trie()：初始化前缀树对象。

void insert(String word)：向前缀树中插入字符串 word。

boolean search(String word)：如果字符串 word 在前缀树中（即之前调用 insert 方法插入过），返回 true；否则返回 false。

boolean startsWith(String prefix)：如果之前已经插入的字符串 word 的前缀之一为 prefix，返回 true；否则返回 false。



# Solution
Trie前缀树：把公共前缀合并到同一条路径上，每个节点代表一个字符

比如先后插入 apple 和 app，整棵树长这样：

root
 └── a
      └── p
           └── p (isEnd=true，对应 "app")
                └── l
                     └── e (isEnd=true，对应 "apple")

关键观察：

根节点不存字符，代表空串。

从根到某节点的路径 = 一个前缀。

一个节点可能同时是"某个词的前缀"和"另一个词的结尾"，所以需要在节点上单独标记 "这里是不是一个单词的结尾"。


# Code

## ACM version

```python
import sys 


class TrieNode:
    def __init__(self):
        self.children = {} # 字符 -> TrieNode
        self.is_end = False # 是否是某个单词的结尾


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root # 先建立根结点
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end = True    # 一个word插入完，最后一个字符的节点标记为单词结尾
        
    def search(self, word):
        node = self.traverse(word)
        return node is not None and node.is_end

    def startsWith(self, prefix):
        return self.traverse(prefix) is not None

    def traverse(self, s): # 沿 s 往下走，走不通返回 None，走通返回最终节点
        node = self.root
        for ch in s:
            if ch not in node.children:
                return None
            node = node.children[ch]
        return node


data = sys.stdin.read().strip().split('\n')
idx = 0
while idx < len(data):
    N = int(data[idx]); idx += 1
    trie = Trie() # 注意只要新建一次实例，不然insert的内容会被丢
    for _ in range(N):
        op = list(data[idx].split())
        idx += 1
        if op[0] == 'insert': # insert 没有返回值
            ans = trie.insert(op[1])
        elif op[0] == 'search':
            ans = trie.search(op[1])
            print('true' if ans == True else 'false')
        elif op[0] == 'startsWith':
            ans = trie.startsWith(op[1])
            print('true' if ans == True else 'false')
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(n)
