# 二叉堆

### 概念

二叉堆，是一种能够动态排序的数据结构，是一种特殊的完全二叉树，具有以下特性:
- 除了最底层外，其他层的节点都是满的，最底层的节点从左到右填充
- 每个节点的值都`大于或等于(大顶堆)`或`小于或等于(小顶堆)`其子节点的值

> 动态排序，即可不断往数据结构里添加或删除元素，数据结构会自动调整元素的位置，使得可以有序地从数据结构中读取元素

### 主要操作

二叉堆的主要操作如下，用以维护二叉堆的性质
- 插入(`insert`)
  - 将新元素添加到堆的末尾（二叉树底层的最右侧）
  - 进行`"上浮"(swim)`操作，将新元素与其父节点比较并交换，直到满足堆的性质
- 删除最大/最小元素(`delete max/min`)
  - 先把堆顶元素删除，用堆的最后一个元素（二叉树底层的最右侧元素）替换根节点
  - 删除最后一个元素
  - 进行`"下沉"(sink)`操作，将根节点与其较大(大顶堆)或较小(小顶堆)的子节点比较并交换，直到满足堆的性质
- 构建堆(`heapify`)
  - 从最后一个非叶子节点开始，对每个节点进行下沉操作，直到根节点

### 实现方式

二叉堆通常使用数组来实现，利用完全二叉树的性质

对于索引为 `i` 的节点：
- 父节点索引: `(i-1) // 2`
- 左子节点索引: `2i + 1`
- 右子节点索引: `2i + 2`

用数组模拟二叉树的原因：
- `链表`节点需一个额外的指针存储相邻节点的地址，所以相对数组，链表的内存消耗会大一些
- 时间复杂度的问题
  - 删除和添加的操作过程，第一步是要找到二叉树最底层的最右侧元素
  - 若需层序遍历或递归遍历二叉树，时间复杂度是 `O(n)`，进而导致操作的时间复杂度退化到 `O(n)`，这显然是不高效的
  - 若用数组来模拟二叉树，就可完美解决这个问题，在 `O(1)` 时间内找到二叉树的底层最右侧节点

![binary_heap1](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/binary_heap1.png)

### 时间复杂度

插入: `O(logn)`

删除最大/最小元素: `O(logn)`

构建堆: `O(n)`

获取最大/最小元素: `O(1)`

### 二叉堆的主要应用

二叉堆的主要应用有两个：
- `优先级队列（Priority Queue）`，一种很有用的数据结构
- `堆排序（Heap Sort）`

### 优先级队列代码实现

```python
class MyPriorityQueue:
    def __init__(self, capacity, cmp=lambda x, y: x < y):
        # 索引 0 空着不用，所以多分配一个空间
        self.heap = [None] * (capacity + 1)
        self._size = 0
        self.cmp = cmp
    
    def size(self):
        return self._size

    def is_empty(self):
        return self._size == 0

    # 父节点的索引
    def parent(self, node):
        return node // 2
    
    # 左子节点的索引
    def left(self, node):
        return node * 2
    
    # 右子节点的索引
    def right(self, node):
        return node * 2 + 1
    
    # 交换数组的两个元素
    def swap(self, i, j):
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
    
    # 查，返回堆顶元素，时间复杂度 O(1)
    def peek(self):
        if self.is_empty():
            raise Exception("Priority queue underflow")
        return self.heap[1]
    
    # 增，向堆中插入一个元素，时间复杂度 O(logn)
    def push(self, x):
        # 扩容
        if self._size == len(self.heap) - 1:
            self.resize(2 * len(self.heap))
        # 把新元素放到最后
        self._size += 1
        self.heap[self._size] = x
        # 然后上浮到正确位置
        self.swim(self._size)
    
    # 删，删除堆顶元素，时间复杂度 O(logn)
    def pop(self):
        if self.is_empty():
            raise Exception("Priority queue underflow")
        res = self.heap[1]
        # 把堆底元素放到堆顶
        self.swap(1, self._size)
        self._size -= 1
        # 然后下沉到正确位置
        self.sink(1)
        # 避免对象游离
        self.heap[self._size + 1] = None
        # 缩容
        if self._size > 0 and self._size == (len(self.heap) - 1) // 4:
            self.resize(len(self.heap) // 2)
        return res
    
    # 上浮操作，时间复杂度是树高 O(logn)
    def swim(self, k):
        while k > 1 and self.cmp(self.heap[self.parent(k)], self.heap[k]):
            self.swap(self.parent(k), k)
            k = self.parent(k)
    
    # 下沉操作，时间复杂度是树高 O(logn)
    def sink(self, k):
        while self.left(k) <= self._size:
            j = self.left(k)
            if j < self._size and self.cmp(self.heap[j], self.heap[j + 1]):
                j += 1
            if not self.cmp(self.heap[k], self.heap[j]):
                break
            self.swap(k, j)
            k = j

    # 调整堆的大小
    def resize(self, capacity):
        assert capacity > self._size
        temp = [None] * capacity
        for i in range(1, self._size + 1):
            temp[i] = self.heap[i]
        self.heap = temp
```

