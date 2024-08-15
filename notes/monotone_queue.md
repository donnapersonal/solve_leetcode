# 单调队列

### 概念

单调队列就是一个`队列`，只是使用了一点巧妙的方法，使得队列中的元素全都是`单调递增（或递减）`

`单调队列`这种结构主要是为了解决下面这个场景：
- 给一个数组 `window`，已知其最值为 `A`，若给 `window` 中添加一个数 `B`，则比较 `A` 和 `B` 就可立即算出新的最值
- 若要从 `window` 数组中减少一个数，就不能直接得到最值，因为若减少的这个数恰好是 `A`，就需遍历 `window` 中的所有元素重新寻找新的最值

> 这个场景不用单调队列似乎也可，如`优先级队列`也是一种特殊的队列，专门用来`动态寻找最值`，创建一个`大（小）顶堆`，就可很快拿到`最大（小）值`
> - 若单纯地维护最值，优先级队列很专业，队头元素就是最值
> - 但优先级队列无法满足标准队列结构`先进先出`的`时间顺序`，因为**优先级队列底层利用`二叉堆`对元素进行动态排序**，元素的出队顺序是元素的大小顺序，和入队的先后顺序完全没有关系
  
所以，需要一种新的队列结构，既**能够维护队列元素「先进先出」的时间顺序，又能够正确维护队列中所有元素的最值**，即`单调队列结构`

`单调队列`主要用来`辅助解决滑动窗口相关的问题`，很多时候把滑动窗口算法作为`双指针`技巧的一部分进行了讲解，但有些稍微复杂的滑动窗口问题不能只靠两个指针来解决，需要上更先进的数据结构

### 时间复杂度和空间复杂度

时间复杂度为 `O(n)`，`nums` 中的每个元素最多也就被 `push` 和 `pop` 各一次，没有任何多余操作，所以整体的复杂度还是 `O(n)`

空间复杂度为 `O(k)`(定义了一个辅助队列)

###  单调队列的实现

```java
// 写法 1
class MyMonotonicQueue {
    LinkedList<Integer> que = new LinkedList<>();
    public void push(int n) {
        // 将小于 n 的元素全部删除
        while (!que.isEmpty() && que.getLast() < n) {
            que.pollLast();
        }
        // 然后将 n 加入尾部
        que.addLast(n);
    }

    public int max() {
        return que.getFirst();
    }

    public void pop(int n) {
        if (n == que.getFirst()) {
            que.pollFirst();
        }
    }
}

// 写法 2
class MyMonotonicQueue {
    Deque<Integer> que = new LinkedList<>();

    public void poll(int val) {
        if (!que.isEmpty() && val == que.peek()) {
            que.poll();
        }
    }

    public void add(int val) {
        while (!que.isEmpty() && val > que.getLast()) {
            que.removeLast();
        }
        que.add(val);
    }

    int peek() {
        return que.peek();
    }
}
```
```python
from collections import deque

class MyMonotonicQueue:
    def __init__(self):
        self.que = deque()

    def poll(self, val):
        if self.que and val == self.que[0]:
            self.que.popleft()

    def add(self, val):
        while self.que and val > self.que[-1]:
            self.que.pop()
        self.que.append(val)

    def peek(self):
        return self.que[0] if self.que else None
```