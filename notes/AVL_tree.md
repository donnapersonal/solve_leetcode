# AVL 树

### 概念

`AVL` 树由 `Adelson-Velsky` 和 `Landis` 在 `1962` 年提出的最先被发明的平衡二叉搜索树

`AVL` 树中任何节点的两棵子树的高度之差的绝对值不超过 `1`，所以它也被称为高度平衡树。`AVL` 树通过严格控制树的高度（通过旋转操作）来保证在最坏情况下依然能提供对数时间复杂度的查找、插入和删除操作，因此其在查找、插入和删除操作的平均和最坏情况下都是 `O(logn)`

`AVL` 的特点：
- 它是一棵空树或它的左右两个子树的高度差的绝对值（平衡因子）不超过 `1`，且左右两个子树都是一棵`平衡二叉树`

注：节点的平衡因子是它的左子树的高度减去右子树的高度，带有平衡因子 `1`、`0` 或 `-1` 的节点被认为是平衡的

**AVL 树的旋转**

`AVL` 树的特点在于`旋转`操作，它能够在不影响二叉树的中序遍历序列的前提下，使失衡节点重新恢复平衡。即旋转操作既能保持`二叉搜索树`的性质，也能使树重新变为`平衡二叉树`

将平衡因子绝对值大于 `1` 的节点称为`失衡节点`，根据节点失衡情况的不同，旋转操作分为四种：`右旋`、`左旋`、`先右旋后左旋`、`先左旋后右旋`

- `右旋`

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
      ...
      # 返回旋转后子树的根节点
      return child
  ```
- `左旋`
  
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
      ...
      # 返回旋转后子树的根节点
      return child
  ```

- `先右旋后左旋`

  ![right_left_rotate](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/right_left_rotate.png)

- `先左旋后右旋`

  ![left_right_rotate](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/left_right_rotate.png)

`旋转`的选择：
- 通过判断失衡节点的平衡因子以及较高一侧子节点的平衡因子的正负号，来确定失衡节点属于哪种情况
  
  |	失衡节点的平衡因子 | 子节点的平衡因子 |	应采用的旋转方法 |
  |	--- |	--- | --- |
  |	> 1（左偏树）| >= 0 | 右旋 |
  |	> 1（左偏树）| < 0 | 先左旋后右旋 |
  |	< -1（右偏树）| <= 0 | 左旋 |
  |	< -1（右偏树）| > 0 | 先右旋后左旋 |

  ![4_operations](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/4_operations.png)

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
