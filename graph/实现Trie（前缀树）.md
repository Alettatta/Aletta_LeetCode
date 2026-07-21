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
我们要存一组字符串，并支持「查找完整单词」和「查找前缀」两种操作。如果用哈希集合，search 很快，但 startsWith 就只能逐个 key 比对前缀，效率不高。Trie 的核心思路是把这组字符串按公共前缀拼成一棵多叉树：从根节点出发，每条边对应一个字符，每条从根到某个节点的路径就对应一个字符串前缀。

比如先后插入 apple 和 app，整棵树长这样：

root
 └── a
      └── p
           └── p (isEnd=true，对应 "app")
                └── l
                     └── e (isEnd=true，对应 "apple")

题目限定字符集只有 a-z，所以每个节点最多 26 个孩子，直接用长度 26 的数组存孩子指针就行，下标 0 对应 a，下标 25 对应 z。

光有树形结构还不够。上面的例子里，插入 apple 之后，沿 a -> p -> p 走确实能到达一个节点，但 app 并没有被真正插入过，它只是 apple 的前缀而已。所以光看「路径能不能走通」无法区分「完整单词」和「某个更长单词的中间路径」。解决办法是给每个节点加一个 isEnd 标记，只有 insert 走到末尾的那个节点才把 isEnd 置为 true。这个标记正是 search 和 startsWith 行为不同的关键：search 要求路径存在且末尾 isEnd 为 true；startsWith 只要路径能走通就行。

这两个查询除了最后一步 isEnd 判断之外完全一样，所以代码里抽出一个 walk 辅助函数：沿字符串向下走，能走通就返回末尾节点，否则返回空。search 在 walk 基础上多检查一次 isEnd，startsWith 直接判空。

insert 也是沿字符串向下走，区别在于路径上遇到不存在的孩子就新建一个节点，最后把末尾节点的 isEnd 置为 true。


# Code

## LC version

```python
class Trie:
    def __init__(self):
        self.children = [None] * 26 # 26 个孩子指针，分别对应字母 a-z
        self.is_end = False # 标记当前节点是否为某个完整单词的结尾


    def insert(self, word: str) -> None: # 向前缀树中插入字符串 word
        node = self
        for ch in word:
            idx = ord(ch) - ord('a')
            if node.children[idx] == None: # 没有这个，要新建
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True
                

    def search(self, word: str) -> bool: # 如果字符串 word 在前缀树中（即之前调用 insert 方法插入过），返回 true；否则返回 false
        node = self._walk(word)
        return node is not None and node.is_end
    def _walk(self, s: str):
        node = self
        for ch in s:
            idx = ord(ch) - ord('a')
            if node.children[idx] == None: # 没有这个，要新建
                return None
            node = node.children[idx]
        return node
        

    def startsWith(self, prefix: str) -> bool: # 如果之前已经插入的字符串 word 的前缀之一为 prefix，返回 true；否则返回 false
        return self._walk(prefix) is not None
```

## ACM version

**ACM 模式的注意点：**

- 需要 import 完整的类（包括 sys、typing 等）
- 数据在标准输入流 stdin 中，全部是原始的文本字符串
- 必须用 print() 手动将结果写到标准输出流 stdout
- 需要写 while 或 for line in sys.stdin 循环处理，直到文件结束（EOF）

```python
import sys

class Trie:
    def __init__(self):
        self.children = [None] * 26 # 26 个孩子指针，分别对应字母 a-z
        self.is_end = False # 标记当前节点是否为某个完整单词的结尾


    def insert(self, word: str) -> None: # 向前缀树中插入字符串 word
        node = self
        for ch in word:
            idx = ord(ch) - ord('a')
            if node.children[idx] == None: # 没有这个，要新建
                node.children[idx] = Trie()
            node = node.children[idx]
        node.is_end = True
                

    def search(self, word: str) -> bool: # 如果字符串 word 在前缀树中（即之前调用 insert 方法插入过），返回 true；否则返回 false
        node = self._walk(word)
        return node is not None and node.is_end
    def _walk(self, s: str):
        node = self
        for ch in s:
            idx = ord(ch) - ord('a')
            if node.children[idx] == None: # 没有这个，要新建
                return None
            node = node.children[idx]
        return node
        

    def startsWith(self, prefix: str) -> bool: # 如果之前已经插入的字符串 word 的前缀之一为 prefix，返回 true；否则返回 false
        return self._walk(prefix) is not None

trie = None
results = []
remaining = -1
for line in sys.stdin:
    line = line.strip()
    if not line:
        continue
    if remaining == -1:
        remaining = int(line)
        trie = Trie()
        results = []
        if remaining == 0:


            remaining = -1
    else:
        op, _, word = line.partition(' ')
        if op == "insert":
            trie.insert(word)
        elif op == "search":
            results.append("true" if trie.search(word) else "false")
        elif op == "startsWith":
            results.append("true" if trie.startsWith(word) else "false")
        remaining -= 1
        if remaining == 0:
            result = "\n".join(results)

            if result:
                print(result)

            remaining = -1
```


# Complexity Analysis
- 时间复杂度：O(n)
- 空间复杂度：O(n)
