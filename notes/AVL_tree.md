# AVL 树

### 概念

`AVL` 树由 `Adelson-Velsky` 和 `Landis` 在 `1962` 年提出的最先被发明的平衡二叉搜索树

`AVL` 树中任何节点的两棵子树的高度之差的绝对值不超过 `1`，所以它也被称为高度平衡树。`AVL` 树通过严格控制树的高度（通过旋转操作）来保证在最坏情况下依然能提供对数时间复杂度的查找、插入和删除操作，因此其在查找、插入和删除操作的平均和最坏情况下都是 `O(logn)`

`AVL` 的特点：
- 它是一棵空树或它的左右两个子树的高度差的绝对值（平衡因子）不超过 `1`，且左右两个子树都是一棵`平衡二叉树`

> 注：`节点的平衡因子`是它的左子树的高度减去右子树的高度，带有平衡因子 `1`、`0` 或 `-1` 的节点被认为是平衡的

求平衡因子：

```python
def height(self, node: TreeNode | None) -> int:
    """获取节点高度"""
    # 空节点高度为 -1 ，叶节点高度为 0
    if node is not None:
        return node.height
    return -1

def update_height(self, node: TreeNode | None):
    """更新节点高度"""
    # 节点高度等于最高子树高度 + 1
    node.height = max([self.height(node.left), self.height(node.right)]) + 1

def balance_factor(self, node: TreeNode | None) -> int:
    """获取平衡因子"""
    # 空节点平衡因子为 0
    if node is None:
        return 0
    # 节点平衡因子 = 左子树高度 - 右子树高度
    return self.height(node.left) - self.height(node.right)
```

### AVL 树的旋转

`AVL` 树的特点在于`旋转`操作，它能够在不影响二叉树的中序遍历序列的前提下，使失衡节点重新恢复平衡。即旋转操作既能保持`二叉搜索树`的性质，也能使树重新变为`平衡二叉树`

将平衡因子绝对值大于 `1` 的节点称为`失衡节点`，根据节点失衡情况的不同，旋转操作分为四种：`右旋`、`左旋`、`先右旋后左旋`、`先左旋后右旋`

**右旋**

![right_rotate](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/right_rotate.png)

实际上需要通过`修改节点指针`来实现

```python
def right_rotate(self, node: TreeNode | None) -> TreeNode | None:
    """右旋操作"""
    child = node.left
    grand_child = child.right
    # 以 child 为原点，将 node 向右旋转
    child.right = node
    node.left = grand_child
    # 更新节点高度
    self.update_height(node)
    self.update_height(child)
    # 返回旋转后子树的根节点
    return child
```

**左旋**
  
![left_rotate](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/right_rotate.png)

右旋和左旋操作在逻辑上是镜像对称的，它们分别解决的两种失衡情况也是对称的。基于对称性，我们只需将右旋的实现代码中的所有的 `left` 替换为 `right`，将所有的 `right` 替换为 `left`，即可得到左旋的实现代码：
```python
def left_rotate(self, node: TreeNode | None) -> TreeNode | None:
    """左旋操作"""
    child = node.right
    grand_child = child.left
    # 以 child 为原点，将 node 向左旋转
    child.left = node
    node.right = grand_child
    # 更新节点高度
    self.update_height(node)
    self.update_height(child)s
    # 返回旋转后子树的根节点
    return child
```

**先右旋后左旋**

![right_left_rotate](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/right_left_rotate.png)

**先左旋后右旋**

![left_right_rotate](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/left_right_rotate.png)

**旋转的选择**

下面 `4` 种情况会破坏 `AVL` 树的特性，可以通过下面更直接的方式来选择合适的操作（`T` 表示平衡因子大于 `1` 的节点）：

|	/ | 状态 |	操作 |
|	--- |	--- | --- |
|	在节点T的左节点(L)的左子树(L)上 |	左左型 | 右旋 |
|	在节点T的左节点(L)的右子树(R)上 |	左右型 | 先左旋后右旋 |
|	在节点T的右节点(R)的右子树(R)上 |	右右型 | 左旋 |
|	在节点T的右节点(R)的左子树(R)上 |	右左型 | 先右旋后左旋 |

![4_operations1]([image.png](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/4_operations1.png))

也可以通过判断失衡节点的平衡因子以及较高一侧子节点的平衡因子的正负号，来确定失衡节点属于哪种情况
  
|	失衡节点的平衡因子 | 子节点的平衡因子 |	应采用的旋转方法 |
|	--- |	--- | --- |
|	> 1（左偏树）| >= 0 | 右旋 |
|	> 1（左偏树）| < 0 | 先左旋后右旋 |
|	< -1（右偏树）| <= 0 | 左旋 |
|	< -1（右偏树）| > 0 | 先右旋后左旋 |

![4_operations2](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/4_operations2.png)

```python
def rotate(self, node: TreeNode | None) -> TreeNode | None:
    """执行旋转操作，使该子树重新恢复平衡"""
    # 获取节点 node 的平衡因子
    balance_factor = self.balance_factor(node)
    # 左偏树
    if balance_factor > 1:
        if self.balance_factor(node.left) >= 0:
            # 右旋
            return self.right_rotate(node)
        else:
            # 先左旋后右旋
            node.left = self.left_rotate(node.left)
            return self.right_rotate(node)
    # 右偏树
    elif balance_factor < -1:
        if self.balance_factor(node.right) <= 0:
            # 左旋
            return self.left_rotate(node)
        else:
            # 先右旋后左旋
            node.right = self.right_rotate(node.right)
            return self.left_rotate(node)
    # 平衡树，无须旋转，直接返回
    return node
```

### AVL 树的常用操作

#### 插入节点

`AVL` 树的节点插入操作与二叉搜索树在主体上类似，区别在于：在 `AVL` 树中插入节点后，从该节点到根节点的路径上可能会出现一系列失衡节点。因此，需要从这个节点开始自底向上执行旋转操作，使所有失衡节点恢复平衡

```python
def insert(self, val):
    """插入节点"""
    self._root = self.insertNode(self._root, val)

def insertNode(self, node: TreeNode | None, val: int) -> TreeNode:
    """递归插入节点（辅助方法）"""
    if node is None:
        return TreeNode(val)
    # 查找插入位置并插入节点
    if val < node.val:
        node.left = self.insertNode(node.left, val)
    elif val > node.val:
        node.right = self.insertNode(node.right, val)
    else:
        # 重复节点不插入，直接返回
        return node
    # 更新节点高度
    self.updateHeight(node)
    # 执行旋转操作，使该子树重新恢复平衡
    return self.rotate(node)
```

#### 删除节点

类似地，在二叉搜索树的删除节点方法的基础上，需要从底至顶执行旋转操作，使所有失衡节点恢复平衡

```python
def remove(self, val: int):
    """删除节点"""
    self._root = self.removeNode(self._root, val)

def removeNode(self, node: TreeNode | None, val: int) -> TreeNode | None:
    """递归删除节点（辅助方法）"""
    if node is None:
        return None
    # 查找节点并删除
    if val < node.val:
        node.left = self.removeNode(node.left, val)
    elif val > node.val:
        node.right = self.removeNode(node.right, val)
    else:
        if node.left is None or node.right is None:
            child = node.left or node.right
            # 子节点数量 = 0 ，直接删除 node 并返回
            if child is None:
                return None
            # 子节点数量 = 1 ，直接删除 node
            else:
                node = child
        else:
            # 子节点数量 = 2 ，则将中序遍历的下个节点删除，并用该节点替换当前节点
            temp = node.right
            while temp.left is not None:
                temp = temp.left
            node.right = self.removeNode(node.right, temp.val)
            node.val = temp.val
    # 更新节点高度
    self.updateHeight(node)
    # 执行旋转操作，使该子树重新恢复平衡
    return self.rotate(node)
```

#### 查找节点

AVL 树的节点查找操作与二叉搜索树一致

给定目标节点值 `num`，可根据二叉搜索树的性质来查找。声明一个节点 `cur`，从二叉树的根节点 `root` 出发，循环比较节点值 `cur.val` 和 `num` 之间的大小关系
- 若 `cur.val < num`，说明目标节点在 `cur` 的右子树中，因此执行 `cur = cur.right` 
- 若 `cur.val > num`，说明目标节点在 `cur` 的左子树中，因此执行 `cur = cur.left` 
- 若 `cur.val = num`，说明找到目标节点，跳出循环并返回该节点

```python
def search(self, num: int) -> TreeNode | None:
    """查找节点"""
    cur = self._root
    # 循环查找，越过叶节点后跳出
    while cur is not None:
        # 目标节点在 cur 的右子树中
        if cur.val < num:
            cur = cur.right
        # 目标节点在 cur 的左子树中
        elif cur.val > num:
            cur = cur.left
        # 找到目标节点，跳出循环
        else:
            break
    return cur
```

二叉搜索树的查找操作与二分查找算法的工作原理一致，循环次数最多为二叉树的高度，当二叉树平衡时使用 `O(logn)` 时间

### AVL 树的常见应用

- 组织和存储大型数据，适用于高频查找、低频增删的场景
- 用于构建数据库中的索引系统
