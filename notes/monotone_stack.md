# 单调栈

### 概念

`单调栈`是一种数据结构，用于保持元素按单调递增或递减顺序排列。它通常用于解决在数组中查找下一个较大或下一个较小元素的问题

关键属性：
- `单调递增堆栈`：栈中的每个元素都大于或等于它之前的元素
- `单调递减堆栈`：栈中的每个元素都小于或等于它之前的元素

### 用法

一些常见问题类型包括：
- 查找下一个更大或下一个更小的元素
  - 通常是`一维数组`，要寻找任一个元素的右边或左边第一个比自己大或小的元素的位置，此时就要想到可以用`单调栈`，时间复杂度为 `O(n)`
  - `单调栈`的本质是`空间换时间`，因为在遍历的过程中需要用一个栈来记录右边第一个比当前元素高的元素，优点是整个数组只需要遍历一次 
- 直方图问题，如在直方图中找到最大的矩形
- 滑动窗口问题

### 注意

在使用单调栈时首先要明确如下几点：
- 单调栈里只需存放元素的下标即可，若需使用对应的元素，直接通过下标就可获取
- 单调栈里元素是递增还是递减?
  - 注意：顺序的描述为从栈头到栈底的顺序
  - 若求一个元素右边第一个更大元素，单调栈就是递增的；若求一个元素右边第一个更小元素，单调栈就是递减的 
- 使用单调栈主要有三个判断条件：
  - 当前遍历的元素 `T[i]` 小于栈顶元素 `T[st.top()]` 的情况
  - 当前遍历的元素 `T[i]` 等于栈顶元素 `T[st.top()]` 的情况
  - 当前遍历的元素 `T[i]` 大于栈顶元素 `T[st.top()]` 的情况

### 单调栈模板

#### 下一个更大的元素

```java
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> stack = new Stack<>(); 
    for (int i = n - 1; i >= 0; i--) {
        while (!stack.isEmpty() && stack.peek() <= nums[i]) {
            stack.pop();
        }
        res[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return res;
}
```

#### 下一个更大或相等的元素

```java
int[] nextGreaterOrEqualElement(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> stack = new Stack<>(); 
    for (int i = n - 1; i >= 0; i--) {
        while (!stack.isEmpty() && stack.peek() < nums[i]) {
            stack.pop();
        }
        res[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return res;
}
```

#### 下一个更小的元素

```java
int[] nextLessElement(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> stack = new Stack<>(); 

    for (int i = n - 1; i >= 0; i--) {
        while (!stack.isEmpty() && stack.peek() >= nums[i]) {
            stack.pop();
        }
        res[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return res;
}
```

#### 下一个更小或相等的元素

```java
int[] nextLessOrEqualElement(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> stack = new Stack<>();
    for (int i = n - 1; i >= 0; i--) {
        while (!stack.isEmpty() && stack.peek() > nums[i]) {
            stack.pop();
        }
        res[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return res;
}
```

#### 上一个更大元素

之前的 `for` 循环都是从数组的尾部开始往栈里添加元素，这样栈顶元素就是 `nums[i]` 之后的元素，因此只要从数组的头部开始往栈里添加元素，栈顶的元素就是 `nums[i]` 之前的元素，即可计算 `nums[i]` 的上一个更大元素

```java
int[] prevGreaterElement(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> stack = new Stack<>(); 
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && stack.peek() <= nums[i]) {
            stack.pop();
        }
        res[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return res;
}
```

#### 上一个更大或相等的元素

```java
int[] prevGreaterOrEqualElement(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> stack = new Stack<>(); 
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && stack.peek() < nums[i]) {
            stack.pop();
        }
        // 现在栈顶就是 nums[i] 前面的更大或相等元素
        res[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return res;
}
```

#### 上一个更小的元素

```java
int[] prevLessElement(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> stack = new Stack<>(); 
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && stack.peek() >= nums[i]) {
            stack.pop();
        }
        // 现在栈顶就是 nums[i] 前面的更小元素
        res[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return res;
}
```

#### 上一个更小或相等的元素

```java
int[] prevLessOrEqualElement(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> stack = new Stack<>(); 
    for (int i = 0; i < n; i++) {
        // 注意不等号
        while (!stack.isEmpty() && stack.peek() > nums[i]) {
            stack.pop();
        }
        // 现在栈顶就是 nums[i] 前面的更小或相等元素
        res[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(nums[i]);
    }
    return res;
}
```
