# 二叉树

### 计算机中的树结构

数据结构中的树，首先是对现实世界中树的一层简化：把树根抽象为`根结点`，树枝抽象为`边`，树枝的两个端点抽象为`结点`，树叶抽象为`叶子结点`，抽象后的树结构如下：

![tree1](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree1.png)

把这棵树颠倒过来就得到了计算机中的树结构：

![tree2](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree2.png)

- `树的层次计算规则`：根结点所在的那层记为第一层，其子结点所在的就是第二层，以此类推 ...
- `结点和树的“高度”计算规则`：叶子结点高度记为 `1`，每向上一层高度就加 `1`，逐层向上累加至目标结点时，所得到的的值就是目标结点的高度
  > 树中结点的最大高度，称为`树的高度`
- `“度”的概念`：一个结点开叉出去多少个子树，就被记为`结点的“度”`，如上图中，根结点的“度”就是 `3`
- `叶子结点`：叶子结点就是度为 `0` 的结点。在上图中，最后一层的结点的度全部为 `0`，所以这一层的结点都是叶子结点

### 二叉树

二叉树是指满足以下要求的树：
- 可以没有根结点，作为一棵空树存在
- 若不是空树，则`必须由根结点、左子树和右子树组成`，且`左右子树都是二叉树`，如下图：

![tree3](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree3.png)

**注意**：二叉树不能被简单定义为每个结点的度都是 `2` 的树

普通的树并不会区分左子树和右子树，但在二叉树中，左右子树的位置是`严格约定、不能交换`的，如上图，`B` 和 `C`、`D` 和 `E`、`F` 和 `G` 是不能互换的

### 常见的二叉树

**满二叉树（Perfect Binary Tree）**

`满二叉树`：若一棵二叉树只有度为 `0` 的结点和度为 `2` 的结点，且度为 `0` 的结点在同一层上，则这棵二叉树为满二叉树

![tree4](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree4.png)

这棵二叉树为`满二叉树`，假设深度为 `h`，则树的高度是从根节点（深度为 `0`）到最底层叶子节点（深度为 `h`）的距离

则`节点总数 = 2^0 + 2^1 + 2^2 + … + 2^h =2^(h+1) - 1`

`满二叉树`广泛应用于数据结构和算法中，如平衡二叉树、二叉堆等

二叉树的退化：
- 完美二叉树是理想情况，可以充分发挥二叉树“分治”的优势
- `链表`则是另一个极端，各项操作都变为线性操作，时间复杂度退化至 `O(n)`

![tuihua](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/tuihua.png)

|	/ | 完美二叉树  | 链表 |
|	--- |	--- |	--- |
|	第 i 层的节点数量）| 2^i - 1 | 1 |
|	高度为 h 的树的叶节点数量 | 2^h | 1 |
|	高度为 h 的树的节点总数 | 2^(h+1) - 1 | h+1 |
|	节点总数为 n 的树的高度 | log(n+1) - 1 | n-1 |

**完全二叉树（Complete Binary Tree）**

`完全二叉树`：
- 一棵完全二叉树是一个二叉树，除了最后一层外的所有层都必须完全填满
- 在最后一层，节点必须从左到右依次填充，没有空隙

> 注意：满二叉树其实是一种特殊的完全二叉树

![tree5](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree5.png)

若最底层为第 `h` 层（`h` 从 `1` 开始），则该层包含 `1 ~ 2^(h-1)` 个节点

`完全二叉树`可以使用数组表示，节点的索引有如下规律：
- 节点索引为 `i`，则左子节点为 `2i+1`，右子节点为 `2i+2`
- 子节点索引为 `i`，则父节点为 `⌊(i−1)/2⌋`

`完全二叉树`的应用：
- `堆`：完全二叉树通常用于实现`堆(Heap)`数据结构，尤其是`二叉堆`，同时保证父子节点的顺序关系。由于其紧凑的结构，可以有效地在数组中表示，便于索引和操作
- 广泛应用：在`优先级队列`、排序算法（如堆排序）中都有广泛应用

**二叉搜索树（Binary Search Tree）**

上面的树都没有数值，而二叉搜索树是有数值的，二叉搜索树是一个`有序树`
- 是一棵空树
- 若它的左子树不空，则`左子树上所有结点的值均小于它的根结点的值`，若它的右子树不空，则`右子树上所有结点的值均大于它的根结点的值` --> `「左小右大」`
- 它的左、右子树也分别为二叉搜索树

![tree6](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree6.png)

`二叉搜索树`的特性：
- 二叉搜索树具有排序的特性：`中序遍历（左-根-右）`二叉搜索树的节点会产生一个递增的序列
- 查找效率：查找操作的平均时间复杂度为 `O(logn)`，但在最坏情况下（退化为链表，如`树不平衡`时），时间复杂度为 `O(n)`
  ![tree11](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/tree11.png)
- 支持多种操作：插入、删除、查找最小值和最大值、查找前驱和后继等操作

`二叉搜索树`的应用：
- 动态集合：快速查找、插入和删除元素
- 排序和范围查询：通过中序遍历可以得到有序序列
- 数据库中的索引：`BST` 结构用于实现高效的查询操作

优点和局限：
- 优点：操作简单，易于实现，且在平衡的情况下，查找、插入、删除等操作效率较高。
- 局限：当输入数据有序或接近有序时，`BST` 可能退化为链表，导致性能下降

**平衡二叉搜索树（Balanced Binary Search Tree）**

`平衡二叉搜索树`：
- 平衡二叉搜索树是一种二叉搜索树，具有“平衡”的特性，通常指的是在插入或删除节点后，树的高度保持在一定范围内，不会偏斜太严重
- 常见的`平衡因子`是保证每个节点的左右子树高度差不超过某个固定值（通常为 `1`）

![tree7](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree7.png)

`平衡二叉搜索树`的常见类型：
- `AVL` 树，又被称为 `AVL(Adelson-Velsky and Landis)`树
  - 它是一棵空树或它的左右两个子树的高度差的绝对值不超过 `1`，且左右两个子树都是一棵`平衡二叉树`
  - 插入和删除节点后，通过`旋转`操作保持平衡
  - 用于组织和存储大型数据，适用于高频查找、低频增删的场景、构建数据库中的索引系统
- `红黑树`
  - 一种近似平衡的二叉搜索树，每个节点有一个颜色属性（红或黑），根节点是黑色，红色节点的子节点必须是黑色（不能有连续的红色节点），每个从根到叶子的路径包含相同数量的黑色节点，通过严格的着色规则和旋转来保持平衡
  - 每个节点是红色或黑色
  - 它的平衡规则相对 `AVL` 树宽松，插入与删除节点所需的旋转操作更少，节点增删操作的平均效率更高，适合频繁插入和删除的场景
  
  详细请看：[【动态图文详解，史上最易懂的红黑树讲解】手写红黑树（Red Black Tree）](https://cloud.tencent.com/developer/article/1739709)
- `树堆（Treap）`
  - 结合了二叉搜索树和堆的性质，利用随机优先级保持树的平衡
- `B` 树和 `B+` 树
  - 广泛用于数据库系统的多叉平衡搜索树，适合存储大量数据
  
  |	B 树 | B+ 树 |
  |	--- |	--- |
  |	所有结点（包括叶子结点）都有数据指针）| 只有叶子结点有数据指针 |
  | 由于叶子结点并非拥有所有的键，搜索通常需更多时间 | 所有键都位于叶子结点，搜索速度更快更准确 |
  | B 树不保留键的副本 | 所有节点的副本都被储存在叶子结点中 |
  | 插入需更多的时间，有时是不可预测的 | 插入更容易，结果总是一样 |
  | 内部结点的删除非常复杂，必须经历很多旋转 | 很容易删除任何结点，所有结点都位于叶子结点 |
  | 叶子结点不会存储为链表结构 | 叶子结点存储为链表结构 |
  | 没有冗余的键 | 可能存在冗余的键 |
  
  详细请看：[B树和B+树详解.md](https://github.com/wardseptember/notes/blob/master/docs/B%E6%A0%91%E5%92%8CB+%E6%A0%91%E8%AF%A6%E8%A7%A3.md)
  
> `C++` 中 `map、set、multimap，multiset` 的底层实现都是`平衡二叉搜索树`，所以 `map、set` 的增删操作时间时间复杂度是 `logn`

### 二叉树的存储方式

二叉树可以`链式存储`，也可以是`顺序存储`
- `顺序存储`的元素在内存是连续分布的，而`链式存储`则是通过指针把分布在各个地址的节点串联一起
- 顺序存储的方式用的是`数组`，链式存储方式用的是`指针`

![tree8](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree8.png)

`顺序存储`

![tree9](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree9.png)

**如何遍历？**
- 若父节点的数组下标是 `i`，则它的左孩子是 `i * 2 + 1`，右孩子是 `i * 2 + 2`

> 用`链式`表示的二叉树更有利于理解，所以`一般都用链式存储二叉树`

### 二叉树的定义

二叉树的定义和链表差不多，但相对于链表，二叉树的节点里多了一个指针，会有两个指针分别指向左右孩子

```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```
```python
class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val
        self.left = left
        self.right = right
```
```js
function TreeNode(val, left, right) {
    this.val = (val===undefined ? 0 : val)
    this.left = (left===undefined ? null : left)
    this.right = (right===undefined ? null : right)
}
```

### 二叉树的遍历方式

二叉树主要有两种遍历方式：
- `广度优先遍历(BFS)`：一层一层的去遍历
- `深度优先遍历(DFS)`：先往深走，遇到叶子节点再往回走

> 这两种遍历是图论中最基本的两种遍历方式

从 `BFS` 和 `DFS` 进一步拓展，才有如下遍历方式：
- 广度优先遍历(`BFS`)
  - 层序遍历（递归法，迭代法）
- 深度优先遍历(`DFS`)
  
  > 深度优先搜索的核心思想：试图穷举所有的完整路径

  - 前序遍历（递归法，迭代法）
  - 中序遍历（递归法，迭代法）
  - 后序遍历（递归法，迭代法）
  
  > 这里前中后，指的是`中间节点的遍历顺序`
  > - 前序遍历：中左右
  > - 中序遍历：左中右
  > - 后序遍历：左右中

  ![tree10](https://github.com/donnapersonal/solve_leetcode/blob/42972e6eac0612a45ea4a79900a87e2d1a0f4d6b/notes/images/tree10.png)

`DFS` 经常会使用`递归`的方式来实现，即实现前中后序遍历，使用`递归`比较方便
- `栈`就是`递归`的一种实现结构，即前中后序遍历的逻辑其实都可以借助`栈`使用`递归`的方式来实现

`BFS` 一般使用`队列`来实现，即`队列先进先出`的特点所决定，因为需要先进先出的结构才能一层一层的来遍历二叉树
- `BFS` 相对 `DFS` 的最主要的区别是：`BFS` 找到的路径`一定是最短的`，但代价就是空间复杂度可能比 `DFS` 大很多

### 二叉树的重要性

比如：`快速排序`就是个二叉树的`前序遍历`，`归并排序`就是个二叉树的`后序遍历`

快速排序的逻辑：若要对 `nums[l..h]` 进行排序，先找一个分界点 `p`，通过交换元素使得 `nums[l..p-1]` 都小于等于 `nums[p]`，且 `nums[p+1..h]` 都大于 `nums[p]`，然后`递归`地去 `nums[l..p-1]` 和 `nums[p+1..h]` 中寻找新的分界点，最后整个数组就排序好了

快速排序的代码框架如下：

```python
def quick_sort(nums: list, l: int, h: int):
    if l >= h:
        return
    # ****** 前序遍历位置 ******
    # 通过交换元素构建分界点 p
    p = partition(nums, l, h)
    # ************************
    
    sort(nums, l, p - 1)
    sort(nums, p + 1, h)
```

归并排序的逻辑：若要对 `nums[l..h]` 进行排序，先对 `nums[l..mid]` 排序，再对 `nums[mid+1..h]` 排序，最后把这两个有序的子数组合并，整个数组就排序好了

归并排序的代码框架如下：

```python
def merge_sort(nums: List[int], l: int, h: int) -> None:
    mid = (l + h) // 2
    # 排序 nums[lo..mid]
    sort(nums, l, mid)
    # 排序 nums[mid+1..h]
    sort(nums, mid + 1, h)

    # ****** 后序位置 ******
    # 合并 nums[l..mid] 和 nums[mid+1..h]
    merge(nums, l, mid, h)
    # *********************
```

从二叉树遍历框架就能扩展出其他算法，由此可见，二叉树的算法思想的运用广泛，甚至可以说只要涉及递归，都可以抽象成二叉树的问题

### 深入理解前中后序

**前中后序是遍历二叉树过程中处理每个节点的三个特殊时间点，绝不仅仅是三个顺序不同的 `List`**
- 前序位置的代码在刚刚进入一个二叉树节点时执行
- 后序位置的代码在将要离开一个二叉树节点时执行
- 中序位置的代码在一个二叉树节点左子树都遍历完，即将开始遍历右子树时执行

![tree12]([tree12.png](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/tree12.png))

> 为什么多叉树没有中序位置？因为二叉树的每个节点只会进行唯一一次左子树切换右子树，而多叉树节点可能有很多子节点，会多次切换子树去遍历，所以多叉树节点没有`「唯一」`的中序遍历位置

因此，二叉树的所有问题都可以归结为：在`前中后序位置`使用巧妙的代码逻辑去达到自己的目的，只需单独思考每个节点应做什么，其他的不用管，抛给二叉树遍历框架，`递归`会在所有节点上做相同的操作

![tree13]([image.png](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/tree13.png))

### 前中后序确定二叉树

`前序和中序`可以唯一确定一棵二叉树，`后序和中序`可以唯一确定一棵二叉树

但`前序和后序`不能唯一确定一棵二叉树！因为没有中序遍历无法确定左右部分，即无法分割

![tree14](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/tree14.png)

`tree1` 的前序遍历是`[1 2 3]`， 后序遍历是`[3 2 1]`；`tree2` 的前序遍历是`[1 2 3]`，后序遍历是`[3 2 1]`

`tree1` 和 `tree2` 的前序和后序完全相同，但不是同一棵树！

所以前序和后序不能唯一确定一棵二叉树！

**后序位置的特殊之处**

有两个问题：
- 若把根节点看做第 1 层，如何打印出每一个节点所在的层数？
  ```python
  def traverse(root, level):
      if root is None:
          return
      # 前序位置
      print(f"Node {root.val} at level {level}")
      traverse(root.left, level + 1)
      traverse(root.right, level + 1)

  traverse(root, 1)
  ```
- 如何打印出每个节点的左右子树各有多少节点？
  ```python
  def count(root):
      if root is None:
          return 0
      leftCount = count(root.left)
      rightCount = count(root.right)
      # 后序位置
      print(f"节点 {root} 的左子树有 {leftCount} 个节点，右子树有 {rightCount} 个节点")
      
      return leftCount + rightCount + 1
  ```

这两个问题的根本区别在于：
- 一个节点在第几层，从根节点遍历过来的过程就能顺带记录，用递归函数的参数就能传递下去；而以一个节点为根的整棵子树有多少个节点，必须遍历完子树后才能数清楚，然后通过递归函数的返回值拿到答案

因此我们可知只有后序位置才能通过返回值获取子树的信息
> 一旦发现题目和子树有关，那大概率要给函数设置合理的定义和返回值，就可以考虑在后序位置处理

注意：利用`后序位置`的题目，一般都使用`「分解问题」`的思路 -- 因为当前节点接收并利用了子树返回的信息，这就意味着把原问题分解成了`当前节点 + 左右子树的子问题`


### 两种解题思路

二叉树题目的递归解法可以分成两类思路：
- 第一类：遍历一遍二叉树得出答案，这类思路对应`回溯算法`
- 第二类：通过分解问题计算出答案，这类思路对应`动态规划`

**以树的视角看动归/回溯/DFS算法的区别和联系**

`DFS` 算法和回溯算法非常类似，只是在细节上有所区别 --> `「做选择」`和`「撤销选择」`到底在 `for` 循环外面还是里面的区别，`DFS` 算法在外面，`回溯`算法在里面

```python
# DFS 算法把「做选择」「撤销选择」的逻辑放在 for 循环外面
def dfs(root):
    if root is None:
        return
    # 做选择
    print("我已经进入节点 %s 啦" % root)
    for child in root.children:
        dfs(child)
    # 撤销选择
    print("我将要离开节点 %s 啦" % root)

# 回溯算法把「做选择」「撤销选择」的逻辑放在 for 循环里面
def backtrack(root):
    if root is None:
        return
    for child in root.children:
        # 做选择
        print("我站在节点 %s 到节点 %s 的树枝上" % (root, child))
        backtrack(child)
        # 撤销选择
        print("我将要离开节点 %s 到节点 %s 的树枝上" % (child, root))
```

动归/DFS/回溯算法都可看做二叉树问题的扩展，只是它们的关注点不同：
- `DFS` 算法属于`遍历`的思路，它的关注点在`单个「节点」`
  
  有一棵二叉树，请写一个 `traverse` 函数，把这棵二叉树上的每个节点的值都加一

  ```python
  def traverse(root):
    if root is None:
        return
    # 遍历过的每个节点的值加一
    root.val += 1
    traverse(root.left)
    traverse(root.right)
  ```

  这就是 `DFS` 算法遍历的思路，它的着眼点永远是在`单一的节点`上，类比到二叉树上就是处理每个`「节点」`

- `回溯`算法属于`遍历`的思路，它的关注点在`节点间的「树枝」`
  
  有一棵二叉树，请用遍历的思路写一个 `traverse` 函数，打印出遍历这棵二叉树的过程
  ```python
  def traverse(root):
      if root is None:
          return
      print("从节点 %s 进入节点 %s" %(root, root.left))
      traverse(root.left)
      print("从节点 %s 回到节点 %s" %(root.left, root))

      print("从节点 %s 进入节点 %s" %(root, root.right))
      traverse(root.right)
      print("从节点 %s 回到节点 %s" %(root.right, root))
  ```

  回溯算法框架：

  ```python
  # 回溯算法框架
  def backtrack(...):
      // base case
      if(...): 
          return
      
      for i in range(...):
          # 做选择
          ...

          # 进入下一层决策树
          backtrack(...)

          # 撤销刚才做的选择
          ...
  ```

  这就是`回溯算法`遍历的思路，它的着眼点永远是在`节点之间移动的过程`，类比到二叉树上就是`「树枝」`

- `动态规划`算法属于`分解`问题`（分治）`的思路，它的关注点在`整棵「子树」`
  
  有一棵二叉树，请用分解问题的思路写一个 `count` 函数，计算这棵二叉树共有多少个节点
  ```python
  def count(root):
      if root is None:
          return 0
      # 当前节点关心的是两个子树的节点总数分别是多少
      # 因为用子问题的结果可以推导出原问题的结果
      leftCount = count(root.left)
      rightCount = count(root.right)
      # 后序位置，左右子树节点数加上自己就是整棵树的节点数
      return leftCount + rightCount + 1
  ```
  这就是`动态规划分解问题`的思路，它的着眼点永远是`结构相同的整个子问题`，类比到二叉树上就是`「子树」`


