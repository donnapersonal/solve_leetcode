# LFU 算法

### 概念

`LFU（Least Frequently Used）`是另一种缓存淘汰算法，用于管理内存或缓存中的数据，每次淘汰那些使用次数最少的数据

其核心思想是：
- 当缓存容量达到上限时，优先移除`访问频率最低`的数据
- 即 `LFU` 会将那些访问次数最少的数据淘汰，而将经常访问的数据保留在缓存中

`LFU` 算法的逻辑不仅考虑了数据的“最近使用”情况（如 `LRU`），还考虑了数据的“访问频率”，适合用于那些特定数据经常被访问的场景

### LFU 实现原理

从实现难度上来说，`LFU 算法 > LRU 算法`，因为 `LRU` 算法相当于把数据`按时间排序`，这个借助链表就能实现，只要一直从链表头部加入元素，越靠近头部的元素就是新的数据，越靠近尾部的元素就是旧的数据，当进行缓存淘汰时只要简单地将尾部的元素淘汰掉即可

而 `LFU` 算法相当于是把数据`按访问频次进行排序`，这个需求没那么简单，且还存在一种情况，若多个数据拥有相同的访问频次，就得删除最早插入的那个数据。即 `LFU` 算法是淘汰访问频次最低的数据，若访问频次最低的数据有多条则需淘汰最旧的数据

### LFU 的优缺点

优点：
- 能够保留最常用的数据：
  - `LFU` 算法的核心思想是保留那些被访问次数最多的数据，淘汰那些被访问次数较少的元素
  - 对于那些访问频率稳定的数据集，`LFU` 能够很好地提高缓存的命中率，从而提升系统性能
- 能够适应长期的访问模式：
  - 若某些数据长期处于频繁访问状态，`LFU` 会不断增加其访问频率，从而让这些数据一直保留在缓存中
  - 适合用于那些访问模式长期不变的应用场景，如某些热门网页、数据集或文件
  - 
- 适合对访问频率敏感的场景：
  - 在某些应用中，用户或系统频繁访问的数据往往是关键数据。`LFU` 能够针对这些频繁访问的数据进行优化，使得关键数据始终在缓存中，从而提高响应速度
  - 
- 与 `LRU` 结合使用的变种：
  - 在实际应用中，`LFU` 可和 `LRU`（最近最少使用）算法结合使用，如 `LFU-K`、`LRU-K` 等混合算法，既考虑访问频率又考虑访问时间，可以进一步提高缓存命中率

缺点：
- 对“突发访问”不敏感：
  - `LFU` 算法容易受到突发访问模式的影响。如果某个数据突然被访问多次（如由于一次突发流量的原因），其频率会被迅速提高，且长时间保留在缓存中
  - 但这种突发访问可能是暂时的，不代表数据的长期重要性，从而导致真正重要的数据被淘汰，影响缓存命中率
- “缓存污染”问题：
  - 在某些场景中，一些数据会短时间内被频繁访问，但之后再也不会被访问（如批量数据预处理时的文件），这些数据的频率会被提升，导致这些不重要的数据保留在缓存中，造成“缓存污染”
  - 缓存被不重要的数据填满后，真正重要的数据将无法进入缓存，从而影响系统性能
- 频率更新的开销：
  - 每次访问一个元素，都需更新其访问频率（从一个频率组移到下一个频率组），这可能涉及多次哈希表查找、删除和插入操作
  - 实现高效的 `LFU` 算法需要复杂的数据结构，如哈希表和双向链表的结合，否则频率更新的开销可能会影响性能
- 难以区分“新”数据和“老”数据：
  - `LFU` 只考虑访问频率，但并不考虑数据的时间因素。因此，某些很久之前访问过很多次但最近不再被访问的数据，可能仍然因为其高频率而长期占据缓存空间，导致新数据无法进入缓存
- 不适合短期访问模式的场景：
  - 如果应用场景中的数据访问模式非常短暂（即很多数据只被访问一次或两次），`LFU` 的优势会大大减弱，因为其主要关注访问频率，而这种频率在短期模式下可能变化过于频繁

### LFU 适用场景

`LFU` 算法通常适用于以下场景：
- `操作系统缓存管理`：用于管理进程或文件的访问缓存，优先保留那些经常被访问的数据
- `数据库系统`： 用于管理索引、表数据等的内存缓存，频繁访问的数据应当优先保留在缓存中
- `内容分发网络 (CDN)`： 在分发网络中，优先保留访问次数较多的内容，提高访问命中率

### LFU 的实现

```python
from collections import OrderedDict

class LFUCache:
    # key 到 val 的映射
    def __init__(self, capacity: int):
        self.keyToVal = {}
        # key 到 freq 的映射
        self.keyToFreq = {}
        # freq 到 key 列表的映射
        self.freqToKeys = {}
        # 记录最小的频次
        self.minFreq = 0
        # 记录 LFU 缓存的最大容量
        self.cap = capacity
        self.size = 0  # Tracks the number of elements

    def get(self, key: int) -> int:
        if key not in self.keyToVal:
            return -1
        # 增加 key 对应的 freq
        self.increaseFreq(key)
        return self.keyToVal[key]

    def put(self, key: int, val: int) -> None:
        if self.cap <= 0:
            return

        # 若 key 已存在，修改对应的 val 即可
        if key in self.keyToVal:
            self.keyToVal[key] = val
            # key 对应的 freq 加一
            self.increaseFreq(key)
            return

        # key 不存在，需要插入
        # 容量已满的话需要淘汰一个 freq 最小的 key
        if self.size >= self.cap:
            self.removeMinFreqKey()

        # 插入 key 和 val，对应的 freq 为 1
        # 插入 KV 表
        self.keyToVal[key] = val
        # 插入 KF 表
        self.keyToFreq[key] = 1
        # 插入 FK 表
        if 1 not in self.freqToKeys:
            self.freqToKeys[1] = OrderedDict()
        self.freqToKeys[1][key] = None
        # 插入新 key 后最小的 freq 肯定是 1
        self.minFreq = 1
        self.size += 1

    def removeMinFreqKey(self):
        # freq 最小的 key 列表
        minFreqKeys = self.freqToKeys[self.minFreq]
        # 其中最先被插入的那个 key 就是该被淘汰的 key
        deletedKey, _ = minFreqKeys.popitem(last=False)

        # 更新 FK 表
        if not minFreqKeys:
            del self.freqToKeys[self.minFreq]

        # 更新 KV 表
        del self.keyToVal[deletedKey]
        # 更新 KF 表
        del self.keyToFreq[deletedKey]
        self.size -= 1

    def increaseFreq(self, key: int):
        freq = self.keyToFreq[key]
        # 更新 KF 表
        self.keyToFreq[key] = freq + 1

        # 更新 FK 表
        # 将 key 从 freq 对应的列表中删除
        del self.freqToKeys[freq][key]
        if not self.freqToKeys[freq]:
            del self.freqToKeys[freq]
            # 如果这个 freq 恰好是 minFreq，更新 minFreq
            if freq == self.minFreq:
                self.minFreq += 1

        # 将 key 加入 freq + 1 对应的列表中
        if freq + 1 not in self.freqToKeys:
            self.freqToKeys[freq + 1] = OrderedDict()
        self.freqToKeys[freq + 1][key] = None
```

