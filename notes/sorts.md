# 排序算法

### 概念

排序算法（`sorting algorithm`）用于对一组数据按照特定顺序进行排列，其有着广泛的应用，因为有序数据通常能够被更高效地查找、分析和处理

评价维度：
- 运行效率：期望排序算法的时间复杂度尽量低，且总体操作数量较少（时间复杂度中的常数项变小），尤其对于大数据量的情况，运行效率显得尤为重要
- 就地性：原地排序通过在原数组上直接操作实现排序，无须借助额外的辅助数组，从而节省内存。通常情况下，原地排序的数据搬运操作较少，运行速度也更快
- 稳定性：稳定排序在完成排序后，相等元素在数组中的相对顺序不发生改变
- 自适应性：能够利用输入数据已有的顺序信息来减少计算量，达到更优的时间效率。自适应排序算法的最佳时间复杂度通常优于平均时间复杂度
- 是否基于比较：基于比较的排序依赖比较运算符（`<`、`=`、`>`）来判断元素的相对顺序，从而排序整个数组，理论最优时间复杂度为 `O(nlogn)`，而非比较排序不使用比较运算符，时间复杂度可达 `O(n)`，但其通用性相对较差

理想排序算法：`运行快、原地、稳定、自适应、通用性好`，但迄今为止尚未发现兼具以上所有特性的排序算法。因此，在选择排序算法时，需要根据具体的数据特点和问题需求来决定

### 冒泡排序（Bubble Sort）

`冒泡排序`通过连续地比较与交换相邻元素实现排序，该过程就像气泡从底部升到顶部一样，因此得名冒泡排序

具体过程：从第一个元素开始，重复比较相邻的两个项，并根据大小交换它们的位置（如左元素 > 右元素就交换二者）；反之不动
- 每一轮操作都会将这一轮中未排序部分中最大的元素放置到数组的末尾，因此假如数组的长度是 `n`，那么当重复完 `n` 轮时，整个数组就有序了

#### 算法流程

设数组的长度为 `n`，冒泡排序的步骤如下：
- 首先，对 `n` 个元素执行“冒泡”，将数组的最大元素交换至正确位置
- 接下来，对剩余 `n-1` 个元素执行“冒泡”，将第二大元素交换至正确位置
- 以此类推，经过 `n-1` 轮“冒泡”后，前 `n-1` 大的元素都被交换至正确位置
- 仅剩的一个元素必定是最小元素，无须排序，因此数组排序完成

```js
// 第一轮排序
[5, 3, 2, 4, 1]
[3, 5, 2, 4, 1]
 ↑  ↑
[3, 2, 5, 4, 1]
    ↑  ↑
[3, 2, 4, 5, 1]
       ↑  ↑
[3, 2, 4, 1, 5]
          ↑  ↑

// 第二轮排序
[2, 3, 4, 1, 5]
 ↑  ↑
[2, 3, 4, 1, 5]
    ↑  ↑
[2, 3, 1, 4, 5]
       ↑  ↑
[2, 3, 1, 4, 5]
          ↑  ↑

// ...
[1, 2, 3, 4, 5]
          ↑  ↑
```

#### 代码实现

```python
def bubbleSort(nums: list[int]):
    n = len(nums)
    for i in range(n):
        for j in range(len - 1 - i):
            if nums[j] > nums[j + 1]:
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
    return nums
```

上面代码的冒泡排序的时间复杂度是 `O(n^2)`，但通过进一步的改进，可做到最好情况下 `O(n)` 的复杂度

> 最好的情况是指：数组本身有序时，只需比较 `n-1` 次，而不需做交换

若某轮“冒泡”中没有执行任何交换操作，说明数组已完成排序，可直接返回结果。因此，可以增加一个标志位 `flag` 来监测这种情况，一旦出现就立即返回，修改的代码如下：

```python
def betterBubbleSort(nums: list[int]):
    n = len(nums)
    for i in range(n):
        flag = False  # 初始化标志位
        # 内循环：将未排序区间 [0, i] 中的最大元素交换至该区间的最右端
        for j in range(n - 1 - i):
            if nums[j] > nums[j + 1]:
                # 交换 nums[j] 与 nums[j + 1]
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
                # 只要发生一次交换，就修改标志位
                flag = True
        # 此轮“冒泡”未交换任何元素，直接跳出
        if not flag:
            return nums
    return nums 
```

### 选择排序（Selection Sort）

`选择排序`每轮从未排序部分中选择最小（或最大）的元素，并将其放在已排序部分的末尾，这个过程重复进行，直到所有元素都排序完成

#### 算法流程

设数组的长度为 `n`，选择排序的算法流程如下：
- 初始状态下，所有元素未排序，即未排序（索引）区间为 `[0, n-1]`
- 选取区间 `[0, n-1]` 中的最小元素，将其与索引 `0` 处的元素交换，完成后，数组前 `1` 个元素已排序
- 选取区间 `[1, n-1]` 中的最小元素，将其与索引 `1` 处的元素交换，完成后，数组前 `2` 个元素已排序
- 以此类推，经过 `n - 1` 轮选择与交换后，数组前 `n - 1` 个元素已排序
- 仅剩的一个元素必定是最大元素，无须排序，因此数组排序完成

```js
[5, 3, 2, 4, 1]
 ↑           ↑
[1, 3, 2, 4, 5]
[1, 3, 2, 4, 5]
    ↑        ↑
[1, 2, 3, 4, 5]
       ↑     ↑
```

> 注意：选择排序是`非稳定排序`，元素 nums[i] 有可能被交换至与其相等的元素的右边，导致相等元素在数组中的相对顺序发生改变

![sort1s](https://github.com/donnapersonal/solve_leetcode/blob/main/notes/images/sort1.png)

#### 代码实现

```python
def selectSort(nums: list[int]):
    n = len(nums)
    for i in range(n - 1):
        # 定义 minIndex，缓存当前区间最小值的索引，注意是索引
        minIndex = i
        for j in range(i + 1, n):
            # 若 j 处的数据项比当前最小值还要小，则更新最小值索引为 j
            if nums[j] < nums[minIndex]:
                minIndex = j  
        # 将该最小元素与未排序区间的首个元素交换
        nums[i], nums[minIndex] = nums[minIndex], nums[i]
```

### 插入排序（Insertion Sort）

`插入排序`是一种简单的排序算法，它的工作原理与手动整理一副牌的过程非常相似，通过构建一个有序的序列，从未排序部分中取出元素，将该元素与其左侧已排序区间的元素逐一比较大小，并将它插入到已排序序列中的正确位置

#### 算法流程

插入排序的整体流程如下：
- 初始状态下，数组的第 `1` 个元素已完成排序
- 选取数组的第 `2` 个元素作，将其插入到正确位置后，数组的前 `2` 个元素已排序
- 选取第 `3` 个元素，将其插入到正确位置后，数组的前 `3` 个元素已排序
- 以此类推，在最后一轮中，选取最后一个元素，将其插入到正确位置后，所有元素排序完成

```js
// 第一轮
[5, 3, 2, 4, 1]

[暂时空出, 5, 2, 4, 1]
当前元素 3

[3, 5, 2, 4, 1]

// ...
[1, 2, 3, 4, 5]
```

> 在有序序列里定位元素位置时，是`从后往前`定位的；只要发现一个比当前元素大的值，就需为当前元素腾出一个新的坑位

> 注意：是`稳定排序`，因为在插入操作过程中，会将元素插入到相等元素的右侧，不会改变它们的顺序

#### 代码实现

```python
def insertSort(nums: list[int]):
    # i 用于标识每次被插入的元素的索引
    for i in range(1, len(nums)):
        # temp 用来保存当前需插入的元素
        temp = nums[i]
        # j 用于帮助 temp 寻找自己应该有的定位
        j = i - 1
        # 判断 j 前面一个元素是否比 temp 大
        while j >= 0 and nums[j] > temp:
            # 若是，则将 j 前面的一个元素后移一位，为 temp 让出位置
            nums[j + 1] = nums[j] 
            j -= 1
        # 循环让位，最后得到的 j 就是 temp 的正确索引
        nums[j + 1] = temp  
```
> 最好的情况下，当输入数组完全有序时，时间复杂度为 `O(n)`

### 插入排序的优势

插入排序的时间复杂度为 `O(n^2)`，而快速排序的时间复杂度为 `O(nlogn)`，尽管插入排序的时间复杂度更高，但`在数据量较小的情况下`，插入排序通常更快
- 快速排序这类算法是基于`分治策略`的排序算法，往往包含更多单元计算操作，在数据量较小时，`n^2` 和 `nlogn` 的数值比较接近，因此复杂度不占主导地位，而是每轮中的单元操作数量起到决定性作用

实际上，许多编程语言（如 `Java`）的内置排序函数采用了`插入排序`，大致思路为：
- 对于`长数组`，采用基于分治策略的排序算法，如快速排序；
- 对于`短数组`，直接使用`插入排序`

经过之前的学习，虽然冒泡排序、选择排序和插入排序的时间复杂度都为 `O(n^2)`，但实际上，插入排序的使用频率显著高于冒泡排序和选择排序，主要有以下原因：
- 冒泡排序基于`元素交换`实现，需借助一个临时变量，共涉及 `3` 个单元操作；而插入排序基于`元素赋值`实现，仅需 `1` 个单元操作
- 而选择排序在任何情况下的时间复杂度都为 `O(n^2)`，若给定一组部分有序的数据，插入排序通常比选择排序效率更高
- 另外，选择排序不稳定，无法应用于多级排序

### 快速排序（Quick Sort）

### 时间复杂度/空间复杂度/稳定性

|	算法 | 时间复杂度 | 空间复杂度 | 稳定性 | 优/缺点 |
| --- | --- | --- | --- | --- |
|	冒泡排序 | 最优 O(n)，最差 O(n^2) | O(1)，原地排序 | 稳定 | 优点：简单易理解；对于少量元素表现较好<br>缺点：性能较差，尤其是对于大数据集 |
|	选择排序 | O(n^2) | O(1)，原地排序 | 不稳定 | 优点：简单直观，不需要额外的空间<br>缺点：即使数组已经排序，算法也不会提前停止，效率低 |
|	选择排序 | O(n^2) | O(1)，原地排序 | 不稳定 | 优点：简单直观，不需要额外的空间<br>缺点：即使数组已经排序，算法也不会提前停止，效率低 |
|	插入排序 | 最优 O(n)，最差 O(n^2) | O(1)，原地排序 | 稳定 | 优点：对于几乎已排序的数据非常有效，在小数据集上性能较好<br>缺点：对于大数据集表现较差 |


