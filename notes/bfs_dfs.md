# BFS/DFS

### 广度优先搜索 - BFS

广度优先搜索（`breadth-first search`, `BFS`），也可称广度优先遍历（`breadth-first traversal`），是一种用于图和树的遍历算法。其以层次的方式从根节点开始，沿着树的宽度逐层遍历树的节点，若所有节点均被访问，则中止

> `BFS` 广泛应用于`最短路径求解`、`搜索问题`和`图的遍历`

层序遍历（`level-order traversal`）：本质上属于 `BFS`，按照层次的顺序，从上到下，从左到右地遍历一个二叉树

![bfs1](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/bfs1.png)

`BFS` 则遵循`逐层推进`的规则，因此 `BFS` 通常借助`队列`来实现，队列遵循`先进先出`的规则，这两者背后的思想是一致的

```python
# 写法 1
# 优点：简单，每次把队头元素拿出来，然后把它的左右子节点加入队列
# 缺点：无法知道当前节点在第几层
def levelOrderTraverse(root):
    if root is None:
        return
    que = deque()
    que.append(root)
    # 初始化一个列表，用于保存遍历序列
    res = []
    while que:
        cur = que.popleft()
        res.append(cur.val)
        # 把 cur 的左右子节点加入队列
        if cur.left is not None:
            res.append(cur.left)
        if cur.right is not None:
            res.append(cur.right)
    return res

# 写法 2
# 可以记录每个节点所在的层数，可以解决诸如二叉树最小深度之类的问题，是最常用的层序遍历写法
def levelOrderTraverse(root):
    if root is None:
        return
    que = deque()
    que.append(root)
    # 记录当前遍历到的层数（根节点视为第 1 层）
    depth = 1
    # 初始化一个列表，用于保存遍历序列
    res = []
    while que:
        n = len(que)
        for i in range(n):
            cur = que.popleft()
            # 访问 cur 节点，同时知道它所在的层数
            res.append(cur.val)
            # 把 cur 的左右子节点加入队列
            if cur.left is not None:
                que.append(cur.left)
            if cur.right is not None:
                que.append(cur.right)
        depth += 1
    return res
```

> - 时间复杂度: `O(n)`，所有节点被访问一次，其中 `n` 为节点数量
> - 空间复杂度为: 在最差情况下即满二叉树时，遍历到最底层之前，队列中最多同时存在 `(n+1)/2` 个节点，占用 `O(n)` 空间

二叉树的层序遍历可以衍生出`多叉树的层序遍历`、`图的 BFS 遍历`以及经典的 `BFS 暴力穷举算法`框架

上面`写法 2`，每向下遍历一层就给 `depth` 加 `1`，可理解为每条树枝的权重是 `1`，二叉树中每个节点的深度其实就是从根节点到这个节点的路径权重和，且同一层的所有节点，路径权重和都是相同的

但假设若每条树枝的权重和可以是任意值，现在层序遍历整棵树，打印每个节点的路径权重和，同一层节点的路径权重和就不一定相同，上面`写法 2` 就无法满足需求

此时可以在写法一的基础上添加一个类，让每个节点自己负责维护自己的路径权重和，代码如下：

```python
# 写法 3
class State:
    def __init__(self, node, depth):
        self.node = node
        self.depth = depth

def levelOrderTraverse(root):
    if root is None:
        return
    que = deque()
    # 根节点的路径权重和是 1
    quee.append(State(root, 1))
    while que:
        cur = q.popleft()
        # 访问 cur 节点，同时知道它的路径权重和
        print(f"depth = {cur.depth}, val = {cur.node.val}")
        # 把 cur 的左右子节点加入队列
        if cur.node.left is not None:
            que.append(State(cur.node.left, cur.depth + 1))
        if cur.node.right is not None:
            que.append(State(cur.node.right, cur.depth + 1))
```

对于普通二叉树，`写法 1/2`即可，但当有不同的权重时，就进化成加权图的问题，此时可以用上述`写法 3`

### 深度优先搜索 - DFS

深度优先搜索（`depth-first search`，`DFS`），也可称深度优先遍历（`depth-first traversal`），是一种用于图和树的遍历算法。其从起始节点开始，沿着每一个可能的路径深入下去，直到无法前进为止，然后回溯到上一个节点，继续探索其他路径

> `DFS` 广泛应用于：
> - 路径查找：找到图中的所有路径或检测是否存在某条路径
> - 拓扑排序：在有向无环图中，DFS 是进行拓扑排序的基础
> - 连通性检测：判断图中连通分量的个数
> - 图的遍历：用于搜索问题，如迷宫/数独问题、图的联通性等

`DFS` 经常会使用`递归`的方式来实现，即实现`前中后序遍历`，使用`递归`比较方便
- `栈`就是`递归`的一种实现结构，即前中后序遍历的逻辑其实都可以借助`栈`使用`递归`的方式来实现


#### 递归法

递归的实现：每次递归调用都会把函数的局部变量、参数值和返回地址等压入调用栈中，递归返回时，从栈顶弹出上一次递归的各项参数，所以递归可以返回上一层位置的原因

```python
# 前序遍历
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        
        def dfs(node):
            if node is None:
                return
            
            res.append(node.val)
            dfs(node.left)
            dfs(node.right)
        dfs(root)
        return res

# 中序遍历
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        
        def dfs(node):
            if node is None:
                return
            
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        dfs(root)
        return res

# 后序遍历
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        
        def dfs(node):
            if node is None:
                return
            
            dfs(node.left)
            dfs(node.right)
            res.append(node.val)

        dfs(root)
        return res
```
> - 时间复杂度: `O(n)`，所有节点被访问一次
> - 空间复杂度: `O(n)`，在最差情况下，即树退化为链表时，递归深度达到 `n`，系统占用 `O(n)` 栈帧空间

#### 迭代法

利用栈的`先进后出`的特点

**前序遍历**

前序遍历是中左右，每次先处理中间节点，则先将根节点放入栈中，然后将右孩子加入栈，再加入左孩子

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 根结点为空则返回空列表
        if not root:
            return []
        stack = [root]
        res = []
        while stack:
            node = stack.pop()
            # 中结点先处理
            res.append(node.val)
            # 右孩子先入栈
            if node.right:
                stack.append(node.right)
            # 左孩子后入栈
            if node.left:
                stack.append(node.left)
        return res
```

**中序遍历**

中序遍历是`左中右`，先访问的是二叉树顶部的节点，然后一层一层向下访问，直到到达树左面的最底部，再开始处理节点（即把节点的数值放进 `res` 数组中），这就造成了处理顺序和访问顺序是不一致的

在使用`迭代法`写中序遍历，就需借用`指针`的遍历来帮助访问节点，`栈`则用来处理节点上的元素

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []

        # 不能提前将root结点加入stack中
        stack = [] 
        res = []
        cur = root
        while cur or stack:
            # 先迭代访问最底层的左子树结点
            if cur:     
                stack.append(cur)
                cur = cur.left		   
            else:		# 到达最左结点后处理栈顶结点 
                cur = stack.pop()
                res.append(cur.val)
                # 取栈顶元素右结点
                cur = cur.right	
        return res
```

**后序遍历**

后序遍历是左右中，只需调整一下先序遍历的代码顺序，就变成中右左的遍历顺序，然后再反转 `res` 数组，输出的结果顺序即左右中

```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        stack = [root]
        res = []
        while stack:
            node = stack.pop()
            # 中结点先处理
            res.append(node.val)
            # 左孩子先入栈
            if node.left:
                stack.append(node.left)
            # 右孩子后入栈
            if node.right:
                stack.append(node.right)
        # 将最终的数组翻转
       return result[::-1]
```

### BFS 和 DFS 的对比

**BFS：**
- 优点
  - 能保证找到最短路径（对于无权图）
  - 更适合解决层次结构的问题
- 缺点
  - 需要更多的内存来保存队列中的节点，特别是当图较宽时内存需求较高，因此空间复杂度可能比 `DFS` 大很多

**DFS：**
- 优点
  - 内存占用较小（特别是递归实现），在路径探索和解决某些复杂问题时非常高效
  - 适合解决路径探索、拓扑排序和检测图中连通性等问题
- 缺点
  - 不能保证找到最短路径
  - 在深度过深的图中可能导致栈溢出（递归实现时）