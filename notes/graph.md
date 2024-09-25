# 图

### 概念

图（`graph`）是一种`非线性数据结构`，由`顶点（vertices）`和`边（edges）`组成，用来表示实体（顶点）及它们之间的关系（边）

图的基本概念：
- `顶点（vertex）`：也称为“节点”，是图中的基本单位，通常用字母或数字表示。顶点表示实体，如在社交网络中，一个人可以被视为一个顶点
- `边（edge）`：连接两个顶点的线，表示两个实体之间的关系。如，在社交网络中边可以表示两个人之间的友谊或联系，而边有以下两种类型：
  - `有向边（directed edge）`：边有方向，表示一种单向的关系，如从顶点 `A` 到顶点 `B` 的关系，用箭头表示
  - `无向边（undirected edge）`：边没有方向，表示一种双向的关系。如 `A` 和 `B` 之间是朋友关系，可以相互访问，用直线表示

`图的典型应用`：地图、网络内容、电路、任务调度、商业交易、配对、计算机网络、软件、社交网络

可以将图抽象地表示为一组顶点 `V` 和一组边 `E` 的集合，如以下示例展示了一个包含 5 个顶点和 7 条边的图

```js
V = {1,2,3,4,5}
E = {(1,2), (1,3), (1,5), (2,3), (2,4), (2,5), (4,5)}
G = {V, E}
```

### 图的分类

**无向图 vs 有向图**

根据边是否具有方向，可分为`无向图（undirected graph）`和`有向图（directed graph）`
- 在无向图中，边表示两顶点之间的“双向”连接关系，如微信或 QQ 中的“好友关系”
- 在有向图中，边具有方向性，即 `A -> B` 和 `B -> A` 两个方向的边是相互独立的，如微博或抖音上的“关注”与“被关注”关系

![graph1](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph1.png)

**连通图 vs 非连通图**

根据所有顶点是否连通，可分为`连通图（connected graph）`和`非连通图（disconnected graph）`
- 对于连通图，从某个顶点出发可以到达其余任意顶点
  
  而对于有向图，连通性可以进一步区分为`强连通`和 `弱连通`：
  - `强连通图`：从图中的任意一个顶点都可通过有向路径到达其他所有顶点
  - `弱连通图`：若忽略方向后，图是连通的
- 对于非连通图，从某个顶点出发至少有一个顶点无法到达

![graph2](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph2.png)

**稀疏图 vs 稠密图**

稀疏图：边的数量远小于顶点数的平方，即 `|E|` 远小于 `|V|^2`，其中 `|E|` 是边数，`|V|` 是顶点数

稠密图：边的数量接近 `|V|^2`，即有很多边

**加权图**

边带有权值（`weight`），权值通常表示顶点之间的距离、费用或其他衡量值

加权图可以是`有向`的，也可以是`无向`的

![graph3](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph3.png)

### 图的术语

图数据结构通常包含以下常用术语：
- `邻接（adjacency）`：当两顶点间存在边相连时，称这两顶点“邻接”
- `路径（path）`：从顶点 `A` 到顶点 `B` 经过的边构成的序列被称为从 `A` 到 `B` 的“路径”
- `度（degree）`：一个顶点拥有的边数。对于有向图，`入度（in-degree）`表示有多少条边指向该顶点，`出度（out-degree）`表示有多少条边从该顶点指出
- `连通分量`：称`无向图中的极大连通子图`为该图的一个`连通分量`
  
  ![graph4](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph4.pngs)

  该无向图中，节点 `1、2、5` 构成的子图就是该无向图中的一个连通分量，该子图所有节点都是相互可达到的

  同理，节点 `3、4、6` 构成的子图也是该无向图中的一个连通分量

  无向图中节点 `3 、4` 构成的子图是该无向图的连通分量吗？- 不是！

  因为必须是`极大联通子图`才能是连通分量，所以此处必须是节点 `3、4、6` 构成的子图才是连通分量
- 强连通分量：称`在有向图中极大强连通子图`为该图的`强连通分量`
  
  ![graph5](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph5.pngs)

  节点 `1、2、3、4、5` 构成的子图是强连通分量，因为这是强连通图也是`极大图`

  节点 `6、7、8` 构成的子图不是强连通分量，因为这不是强连通图，节点 `8` 不能达到节点 `6`

### 图的表示方式

在实际编程中，图有以下几种常见的表示方式：
- 邻接矩阵（adjacency matrix）
  
  使用`二维数组`来表示图结构，其是从节点的角度来表示图，有多少节点就申请多大的二维数组

  设图的顶点数量为 `n`，这里使用一个 `n x n` 大小的矩阵来表示图，每行（列）代表一个顶点，矩阵元素代表边，用 `1`（或权重值）或 `0` 表示两个顶点之间是否存在边

  如设邻接矩阵为 `M`、顶点列表为 `V`，则矩阵元素 `M[i, j] = 1` 表示顶点 `V[i]` 到顶点 `V[j]` 之间存在边，反之 `M[i, j] = 0` 表示两顶点之间无边

  ![graph6](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph6.png)

  邻接矩阵具有以下特性：
  - 顶点不能与自身相连，因此邻接矩阵主对角线元素没有意义
  - 对于无向图，两个方向的边等价，此时邻接矩阵关于主对角线对称
  - 将邻接矩阵的元素从 `1` 和 `0` 替换为权重，则可表示有权图
  
  邻接矩阵的优点：
  - 表达方式简单，易于理解
  - 检查任意两个顶点间是否存在边的操作非常快
  - 适合稠密图，在边数接近顶点数平方的图中，邻接矩阵是一种空间效率较高的表示方法
  
  缺点：遇到稀疏图，会导致申请过大的二维数组造成空间浪费且遍历边时需要遍历整个 `n * n` 矩阵，造成时间浪费

- 邻接表（adjacency list）
  
  邻接表使用`数组 + 链表`的方式来表示。邻接表是从边的数量来表示图，有多少边才会申请对应大小的链表

  数组的每个元素都是一个链表或数组，存储与该顶点相邻的其他顶点，即第 `i` 个链表对应顶点，其中存储了该顶点的所有邻接顶点（与该顶点相连的顶点）

  ![graph7](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph7.png)

  邻接表的优点：
  - 对于稀疏图的存储，只需存储边，空间利用率高，空间复杂度为 `O(V + E)`
  - 遍历节点连接情况相对容易

  缺点：
  - 检查任意两个节点间是否存在边，效率相对低，需要遍历链表，需要 `O(V)` 时间，`V` 表示某节点连接其他节点的数量
  - 实现相对复杂，不易理解
  
  邻接表结构与哈希表中的“链式地址”非常相似，因此也可以采用类似的方法来优化效率。如当链表较长时，可将链表转化为 `AVL` 树或`红黑树`，从而将时间效率从 `O(n)` 优化至 `O(logn)`；还可把链表转换为哈希表，从而将时间复杂度降至 `O(1)`

### 图的基础操作

**基于邻接矩阵的实现**

给定一个顶点数量为 `n` 的无向图，主要有以下几种操作：
- 添加或删除边：直接在邻接矩阵中修改指定的边即可，使用 `O(1)` 时间。若是无向图，需同时更新两个方向的边
- 添加顶点：在邻接矩阵的尾部添加一行一列，并全部填 `0` 即可，使用 `O(n)` 时间
- 删除顶点：在邻接矩阵中删除一行一列。当删除首行首列时达到最差情况，需要将 `(n-1)^2` 个元素“向左上移动”，从而使用 `O(n^2)` 时间
- 初始化：传入 `n` 个顶点，初始化长度为 `n` 的顶点列表，使用 `O(n)` 时间；初始化 `nxn` 大小的邻接矩阵，使用 `O(n^2` 时间

**基于邻接表的实现**

设无向图的顶点总数为 `n`、边总数为 `m`，主要有以下几种操作：
- 添加边：在顶点对应链表的末尾添加边即可，使用 `O(1)` 时间。在无向图，需同时添加两个方向的边
- 删除边：在顶点对应链表中查找并删除指定边，使用 `O(m)` 时间。在无向图中，需要同时删除两个方向的边
- 添加顶点：在邻接表中添加一个链表，并将新增顶点作为链表头节点，使用 `O(1)` 时间
- 删除顶点：需遍历整个邻接表，删除包含指定顶点的所有边，使用 `O(n+m)` 时间
- 初始化：在邻接表中创建 `n` 个顶点和 `2m` 条边，使用 `O(n+m)` 时间

**有向加权图**

```python
# 邻接矩阵实现
class WeightedDigraph:
    # 存储相邻节点及边的权重
    class Edge:
        def __init__(self, to, weight):
            self.to = to
            self.weight = weight

  def __init__(self, n):
      # 邻接矩阵，matrix[from][to] 存储从节点 from 到节点 to 的边的权重
      # 0 表示没有连接
      self.matrix = [[0] * n for _ in range(n)]
    
  # 增，添加一条带权重的有向边，复杂度 O(1)
  def addEdge(self, from_node, to_node, weight):
      self.matrix[from_node][to_node] = weight
  
  # 删，删除一条有向边，复杂度 O(1)
  def removeEdge(self, from_node, to_node):
      self.matrix[from_node][to_node] = 0
  
  # 查，判断两个节点是否相邻，复杂度 O(1)
  def hasEdge(self, from_node, to_node):
      return self.matrix[from_node][to_node] != 0
  
  # 查，返回一条边的权重，复杂度 O(1)
  def weight(self, from_node, to_node):
      return self.matrix[from_node][to_node]
  
  # 查，返回某个节点的所有邻居节点，复杂度 O(V)
  def neighbors(self, v):
      res = []
      for i in range(len(self.matrix[v])):
          if self.matrix[v][i] > 0:
              res.append(self.Edge(i, self.matrix[v][i]))
      
      return res

# 邻接表阵实现
class WeightedDigraph:
    # 存储相邻节点及边的权重
    class Edge:
        def __init__(self, to: int, weight: int):
            self.to = to
            self.weight = weight
    
    def __init__(self, n: int):
        # 这里简单起见，建图时要传入节点总数，可以优化
        # 如把 graph 设置为 Map<Integer, List<Edge>>，就可动态添加新节点
        self.graph = [[] for _ in range(n)]
    
    # 增，添加一条带权重的有向边，复杂度 O(1)
    def addEdge(self, from_node: int, to_node: int, weight: int):
        self.graph[from_node].append(self.Edge(to_node, weight))
    
    # 删，删除一条有向边，复杂度 O(V)
    def removeEdge(self, from_node: int, to_node: int):
        self.graph[from_node] = [e for e in self.graph[from_node] if e.to != to_node]
    
    # 查，判断两个节点是否相邻，复杂度 O(V)
    def hasEdge(self, from_node: int, to_node: int) -> bool:
        for node in self.graph[from_node]:
            if node.to == to_node:
                return True
        return False

    # 查，返回一条边的权重，复杂度 O(V)
    def weight(self, from_node: int, to_node: int) -> int:
        for node in self.graph[from_node]:
            if node.to == to_node:
                return node.weight
        raise ValueError("No such edge")
    
    # 上面的 hasEdge、removeEdge、weight 方法遍历 List 的行为是可优化的
    # 如用 Map<Integer, Map<Integer, Integer>> 存储邻接表
    # 这样就可以避免遍历 List，复杂度就能降到 O(1)

    # 查，返回某个节点的所有邻居节点，复杂度 O(1)
    def neighbors(self, v: int):
        return self.graph[v]
```

**有向无权图**

直接复用上面的 `WeightedDigraph` 类即可，把 `addEdge` 方法的权重参数默认设置为 `1`，比较简单


**无向加权图**

等同于`双向的有向加权图`，可直接复用上面用`邻接表/领接矩阵`实现的 `WeightedDigraph` 类即可，只是在增加边时要同时添加两条边

```python
class WeightedUndigraph:
    def __init__(self, n):
        self.graph = WeightedDigraph(n)
    
    # 增，添加一条带权重的无向边
    def addEdge(self, from_node, to_node, weight):
        self.graph.addEdge(from_node, to_node, weight)
        self.graph.addEdge(to_node, from_node, weight)
    
    # 删，删除一条无向边
    def removeEdge(self, from_node, to_node):
        self.graph.removeEdge(from_node, to_node)
        self.graph.removeEdge(to_node, from_node)

    # 查，判断两个节点是否相邻
    def hasEdge(self, from_node, to_node):
        return self.graph.hasEdge(from_node, to_node)
    
    # 查，返回一条边的权重
    def weight(self, from_node, to_node):
        return self.graph.weight(from_node, to_node)
    
    # 查，返回某个节点的所有邻居节点
    def neighbors(self, v):
        return self.graph.neighbors(v)
```

**无向无权图**

直接复用上面的 `WeightedUndigraph` 类即可，把 `addEdge` 方法的权重参数默认设置为 `1`，比较简单

### 图的遍历方式

图的遍历方式基本是两大类：
- 深度优先搜索（`dfs`）
- 广度优先搜索（`bfs`）

`dfs` 和 `bfs` 可以在不同的数据结构上进行搜索，在二叉树章节里是在二叉树这样的数据结构上搜索。而在图论章节，则是在图（邻接表或邻接矩阵）上进行搜索

**BFS 的实现**

`BFS` 是一种由近及远的遍历方式，从某个节点出发，始终优先访问距离最近的顶点，并一层层向外扩张。从左上角顶点出发，首先遍历该顶点的所有邻接顶点，然后遍历下一个顶点的所有邻接顶点... 以此类推，直至所有顶点访问完毕

![graph8](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph8.png)

`BFS` 通常借助`队列`来实现，队列具有`“先入先出”`的性质，这与 `BFS` 的`“由近及远”`的思想异曲同工
- 将遍历起始顶点加入队列，并开启循环
- 在循环的每轮迭代中，弹出队首顶点并记录访问，然后将该顶点的所有邻接顶点加入到队列尾部
- 循环第 2 步，直到所有顶点被访问完毕后结束

这里为了防止重复遍历顶点，借助了一个哈希集合 `visited` 来记录哪些节点已被访问
```python
def graphBFS(graph, start):
  res = []
  visited = set[Vertex]([start])
  # 队列用于实现 BFS
  que = deque[Vertex]([start])
  # 以顶点 start 为起点，循环直至访问完所有顶点
  while que:
    cur = que.popleft()  # 队首顶点出队
    res.append(cur)  # 记录访问顶点
    # 遍历该顶点的所有邻接顶点
    for v in graph.v_list[cur]:
      if v in visited:
        continue  # 跳过已被访问的顶点
      que.append(v)  # 只入队未访问的顶点
      visited.add(v)  # 标记该顶点已被访问
    
  # 返回顶点遍历序列
  return res
```
- 时间复杂度：所有顶点都会入队并出队一次，使用 `O(|V|)` 时间；在遍历邻接顶点的过程中，由于是无向图，因此所有边都会被访问 `2` 次，使用 `O(2|E|)` 时间；总体使用 `O(|V| + |E|)` 时间
- 空间复杂度：列表 `res`，哈希集合 `visited`，队列 `que` 中的顶点数量最多为 `|V|`，使用 `O(|V|)` 空间

**DFS 的实现**

深度优先遍历是一种优先走到底、无路可走再回头的遍历方式。从左上角顶点出发，访问当前顶点的某个邻接顶点，直到走到尽头时返回，再继续走到尽头并返回... 以此类推，直至所有顶点遍历完成s

![graph9](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/graph9.png)

这种“走到尽头再返回”的算法范式通常基于`递归`来实现。与广度优先遍历类似，在深度优先遍历中也需要借助一个哈希集合 `visited` 来记录已被访问的顶点，以避免重复访问顶点

```python
def dfs(graph, visited, res, vet):
    res.append(vet)  # 记录访问顶点
    visited.add(vet)  # 标记该顶点已被访问
    # 遍历该顶点的所有邻接顶点
    for v in graph.v_list[vet]:
        if v in visited:
            continue  # 跳过已被访问的顶点
        # 递归访问邻接顶点
        dfs(graph, visited, res, v)

def graphDFS(graph, start):
    res = []
    # 哈希集合，用于记录已被访问过的顶点
    visited = set[Vertex]()
    dfs(graph, visited, res, start)
    return res
```
- 时间复杂度：所有顶点都会入队并出队一次，使用 `O(|V|)` 时间；在遍历邻接顶点的过程中，由于是无向图，因此所有边都会被访问 `2` 次，使用 `O(2|E|)` 时间；总体使用 `O(|V| + |E|)` 时间
- 空间复杂度：列表 `res`，哈希集合 `visited`，队列 `que` 中的顶点数量最多为 `|V|`，使用 `O(|V|)` 空间

### 图的常见应用

|	/ |	顶点 |	边 | 图的计算问题 | 
|	--- |	--- | --- | --- |
|	社交网络 |	用户 | 好友关系 | 潜在好友推荐 |
|	地铁线路 |	站点 | 站点间的连通性 | 最短路线推荐 |
|	太阳系 |	星体 | 星体间的万有引力作用 | 行星轨道计算 |