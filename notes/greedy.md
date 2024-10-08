# 贪心算法

### 概念

贪心算法（`greedy algorithm`）是一种常见的解决优化问题的算法，其希望**通过每一步的局部最优选择，最终能够得到全局最优解**

贪心算法的基本思想：在问题的每个决策阶段，总是做出在当前看来最优的选择，而不考虑未来的全局可能性，即贪心地做出局部最优的决策，以期获得全局最优解

贪心算法的特点：
- 局部最优选择：在问题的每一步，选择当前状态下最优的解
- 全局最优期望：通过不断进行局部最优的选择，最终得到全局最优解
- 无法回溯：一旦做出了某个选择，不能改变这个选择，也不能重新计算之前的状态，即贪心算法不会考虑过去的决策，而是一路向前地进行贪心选择，然后不断缩小问题范围，直至问题被解决
- 适合贪心选择性问题：只有满足贪心选择性和最优子结构性质的问题，才能使用贪心算法得到正确的全局最优解

### 应用场景

贪心算法的使用条件更加苛刻，其主要关注问题的两个性质：
- 贪心选择性质：问题的解可以通过选择局部最优解一步步构建全局最优解
- 最优子结构：问题的最优解可以通过子问题的最优解来构造

### 优点与缺点

优点：
- 简单高效：贪心算法通常很简单，时间复杂度较低，能快速得到问题的解
- 局部最优：适用于某些问题时，它可以保证全局最优解

缺点：
- 局部最优不保证全局最优：对于某些问题，贪心算法可能无法得到全局最优解，因为它只关注当前的最优选择，而没有考虑后续步骤的影响
- 适用问题有限：贪心算法并不适用于所有问题，只有满足贪心选择性质的问题才能使用它

![greedy1](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/greedy1.png)

因此，一般情况下，贪心算法的适用情况分以下两种：
- 可保证找到最优解：贪心算法在这种情况下往往是最优选择，因为它往往比回溯、动态规划更高效
- 可找到近似最优解：贪心算法在这种情况下也是可用的。对于很多复杂问题来说，寻找全局最优解非常困难，能以较高效率找到次优解也是非常不错的

### 解题步骤

贪心算法一般分为如下四步：
- 将问题分解为若干个子问题
- 找出适合的贪心策略
- 求解每个子问题的最优解
- 将局部最优解堆叠成全局最优解

为了保证正确性，通常需要对贪心策略进行严谨的数学证明，证明问题具有贪心选择性质和最优子结构，这时可能需要用到数学证明，如`归纳法`或`反证法`等

### 典型例题

贪心算法常常应用在满足贪心选择性质和最优子结构的优化问题中，以下列举了一些典型的贪心算法问题
- `硬币找零`问题：在某些硬币组合下，贪心算法总是可以得到最优解
- `区间调度`问题：假设有一些任务，每个任务在一段时间内进行，目标是完成尽可能多的任务。若每次都选择结束时间最早的任务，则贪心算法就可以得到最优解
- `分数背包`问题：给定一组物品和一个载重量，目标是选择一组物品，使得总重量不超过载重量且总价值最大。若每次都选择性价比最高（价值 / 重量）的物品，则贪心算法在一些情况下可以得到最优解
- `股票买卖`问题：给定一组股票的历史价格，可以进行多次买卖，但若已经持有股票，则在卖出之前不能再买，目标是获取最大利润
- `霍夫曼编码`：霍夫曼编码是一种用于`无损数据压缩`的贪心算法。通过构建霍夫曼树，每次选择出现频率最低的两个节点合并，最后得到的霍夫曼树的带权路径长度（编码长度）最小
- `Dijkstra` 算法：它是一种解决给定源顶点到其余各顶点的最短路径问题的贪心算法

