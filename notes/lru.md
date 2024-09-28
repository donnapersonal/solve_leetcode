# LRU 算法

### 概念

`LRU（Least Recently Used）` 是一种缓存淘汰算法，用于管理内存或缓存中的数据

`LRU` 缓存淘汰算法就是一种常用策略。我们认为最近使用过的数据应该是是`有用的`，很久都没用过的数据应该是无用的，内存满了就优先删那些很久没用过的数据

核心思想是：当缓存空间不足时，优先移除`最久未使用`的数据项，即 `LRU` 会将最近使用过的数据保留，而将最近一段时间内没有使用的数据淘汰掉

### LRU 实现原理

实现 `LRU` 的核心在于如何高效地维护访问的顺序，并在数据被访问时、插入时、删除时能够快速调整顺序

`LRU` 缓存算法的核心数据结构就是`哈希链表`（双向链表和哈希表的结合体），这个数据结构长这样：

![lru1](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/lru1.png)


若每次默认从链表尾部添加元素，则越靠近尾部的元素就是最近使用的，越靠头部的元素就是最久未使用的

对于某一个 `key`，可以通过哈希表快速定位到链表中的节点，从而取得对应 `val`

另外，链表支持在任意位置快速插入和删除，则只要改改指针即可，只是传统的链表无法按照索引快速访问某一个位置的元素，因此这里借助哈希表，可以通过 `key` 快速映射到任意一个链表节点，然后进行插入和删除

> 总结：使用哈希链表，借助链表的有序性使得链表元素维持插入顺序，同时借助哈希映射的快速访问能力使得可以在 `O(1)` 时间访问链表的任意元素

> 为什么要是双向链表，单链表行不行？
> - 因为需要删除操作，删除一个节点不光要得到该节点本身的指针，也需要得到其前驱节点的指针，而双向链表才能支持直接查找前驱，保证操作的时间复杂度 `O(1)`
>
> 既然哈希表中已存了 `key`，为什么链表中还要存 `key` 和 `val` 呢？
> - 当缓存容量已满，不仅仅要删除最后一个节点，还要把 `map` 中映射到该节点的 `key` 同时删除，而这个 `key` 只能由节点得到
> - 若节点结构中只存储 `val`，则无法得知 `key` 是什么，就无法删除 `map` 中的键，造成错误

### LRU 适用场景

`LRU` 算法主要用于需要频繁访问缓存的场景，如：
- `操作系统的内存管理`：用于管理页缓存，在内存不够时决定淘汰哪个页面
- `数据库系统`：用于管理内存中的索引和表数据
- `Web 缓存`： 用于存储网页内容，快速响应用户请求

### LRU 算法的优缺点

优点:
- 能够有效地利用缓存空间，保持最近使用的数据项，提高命中率
- 能够在常数时间内完成 `get` 和 `put` 操作，非常高效
  
缺点:
- 维护链表和哈希表的数据结构相对复杂
- 当数据访问模式非常随机且无规律时，`LRU` 的命中率可能不理想

### 完整实现

#### 使用手写 doubleList
```python

class Node:
    def __init__(self, k, v):
        self.key = k
        self.val = v
        self.next = None
        self.prev = None

class DoubleList:
    def __init__(self):
        # 头尾虚节点
        self.head = Node(0, 0)
        self.tail = Node(0, 0)
        # 链表元素数
        self._size = 0

        # 初始化双向链表的数据
        self.head.next = self.tail
        self.tail.prev = self.head

    # 在链表尾部添加节点 x，时间 O(1)
    def addLast(self, x):
        x.prev = self.tail.prev
        x.next = self.tail
        self.tail.prev.next = x
        self.tail.prev = x
        self._size += 1

    # 删除链表中的 x 节点（x 一定存在）
    # 由于是双链表且给的是目标 Node 节点，时间 O(1)
    def remove(self, x):
        x.prev.next = x.next
        x.next.prev = x.prev
        self._size -= 1

    # 删除链表中第一个节点，并返回该节点，时间 O(1)
    def removeFirst(self):
        if self.head.next == self.tail:
            return None
        first = self.head.next
        self.remove(first)
        return first

    # 返回链表长度，时间 O(1)
    def size(self):
        return self._size
    

class LRUCache:
    def __init__(self, capacity: int):
        # key -> Node(key, val)
        self.map = {}
        # Node(k1, v1) <-> Node(k2, v2)...
        self.cache = DoubleList()
        # 最大容量
        self.cap = capacity

    def get(self, key: int) -> int:
        if key not in self.map:
            return -1
        
        # 将该数据提升为最近使用的
        self.makeRecently(key)
        return self.map[key].val

    def put(self, key: int, val: int) -> None:
        if key in self.map:
            # 删除旧的数据
            self.deleteKey(key)
            # 新插入的数据为最近使用的数据
            self.addRecently(key, val)
            return
        
        if self.cap == self.cache.size():
            # 删除最久未使用的元素
            self.removeLeastRecently()
        # 添加为最近使用的元素
        self.addRecently(key, val)

    def makeRecently(self, key: int):
        x = self.map[key]
        # 先从链表中删除这个节点
        self.cache.remove(x)
        # 重新插到队尾
        self.cache.addLast(x)

    def addRecently(self, key: int, val: int):
        x = Node(key, val)
        # 链表尾部就是最近使用的元素
        self.cache.addLast(x)
        # 别忘了在 map 中添加 key 的映射
        self.map[key] = x

    def deleteKey(self, key: int):
        x = self.map[key]
        # 从链表中删除
        self.cache.remove(x)
        # 从 map 中删除
        self.map.pop(key)

    def removeLeastRecently(self):
        # 链表头部的第一个元素就是最久未使用的
        deletedNode = self.cache.removeFirst()
        # 同时别忘了从 map 中删除它的 key
        deletedKey = deletedNode.key
        self.map.pop(deletedKey)
```

#### 使用内置数据结构

`JAVA` 可以使用内置的 `LinkedHashMap`

`JS` 中可以使用 `new Map()`

`Python` 中可以使用内置的 `collections.OrderedDict` 来替代手写的 `DoubleList` 数据结构，从而实现同样的 `LRU` 缓存功能

`OrderedDict` 保证了插入的顺序，且可以在常数时间内完成插入、删除和移动到末尾的操作，非常适合用于 `LRU` 缓存的实现

在实现中，`OrderedDict` 的特性如下：
- 当插入一个键时，`OrderedDict` 会将该键自动添加到末尾，表示最近使用
- 通过 `popitem(last=False)` 方法，可以移除最前面的键（表示最久未使用）
- 通过 `move_to_end(key)` 方法，可以将指定键移动到末尾，表示最近使用