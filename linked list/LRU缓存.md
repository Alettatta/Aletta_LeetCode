# Problem
https://labuladong.online/zh/problem/leetcode/lru-cache/description/


# Problem Description
请你设计并实现一个满足 LRU（最近最少使用）缓存约束的数据结构。

实现 LRUCache 类：

LRUCache(int capacity)：以正整数作为容量 capacity 初始化 LRU 缓存。

int get(int key)：如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1。

void put(int key, int value)：如果关键字 key 已经存在，则变更其数据值 value；如果不存在，则向缓存中插入该组 key-value。如果插入操作导致关键字数量超过 capacity，则应该 逐出 最久未使用的关键字。

函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。

核心代码模式下，你需要实现 LRUCache 类，对外提供构造函数、get 与 put 三个方法。判题端会按调用序列依次执行你实现的方法，并收集所有 get 调用的返回值。


# Key Points
LRU = Least Recently Used（最近最少使用），就像你的手机应用后台管理：手机最多同时运行5个应用（容量capacity=5）——你最近打开的应用排在前面，当打开第6个应用时，最久没用的那个应用会被自动关闭

需要实现的功能belike：
```python
class LRUCache:
    def __init__(self, capacity: int):
        # 初始化缓存，设置最大容量
        
    def get(self, key: int) -> int:
        # 根据key获取value，如果不存在返回-1
        # 使用一次就算"最近使用"了，要更新顺序
        
    def put(self, key: int, value: int) -> None:
        # 存入键值对
        # 如果key已存在：更新value，并标记为"最近使用"
        # 如果key不存在：插入，如果满了就删除"最久未使用"的
```

要让 put 和 get 方法的时间复杂度为 O(1)，我们可以总结出 cache 这个数据结构必要的条件：

1、显然 cache 中的元素必须有时序，以区分最近使用的和久未使用的数据，当容量满了之后要删除最久未使用的那个元素腾位置。

2、我们要在 cache 中快速找某个 key 是否已存在并得到对应的 val；

3、每次访问 cache 中的某个 key，需要将这个元素变为最近使用的，也就是说 cache 要支持在任意位置快速插入和删除元素。


# Solution
**哈希表**查找快，但是数据无固定顺序；**双链表**有顺序之分，插入删除快，但是查找慢，所以结合二者的长处，可以形成一种新的数据结构：哈希链表 LinkedHashMap：




# Code

## LC version

```python
class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.cache = collections.OrderedDict()
        # 有序字典记住插入顺序，新插入的放最后
        # d['a'] = 1  # 顺序: ['a']
        # d['b'] = 2  # 顺序: ['a', 'b']
        # d['c'] = 3  # 顺序: ['a', 'b', 'c']

    def get(self, key: int) -> int:
        # 如果查找值不存在
        if key not in self.cache:
            return -1
        # 不然就放到最近使用
        else:
            self.makeRecently(key)
            return self.cache[key]
            
    def makeRecently(self, key: int) -> None:
        value = self.cache.pop(key) # 删除整个键值对
        self.cache[key] = value # 重新插入到最后，变为最近使用

    def put(self, key: int, value: int) -> None:
        # 如果已经在cache里面，要变成最近使用
        if key in self.cache:
            self.cache[key] = value
            self.makeRecently(key)
            return

        # 如果已经满了
        if len(self.cache) >= self.cap:
            oldestKey = next(iter(self.cache)) # iter创建迭代器，next返回第一个元素
            self.cache.pop(oldestKey)
            
        # 如果不在cache里面，无论是否满员都要执行
        self.cache[key] = value
```



# Complexity Analysis
- 时间复杂度：O(1)
- 空间复杂度：O(capacity)
