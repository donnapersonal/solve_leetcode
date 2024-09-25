# 滑动窗口

### 概念

`滑动窗口`是一种常用于解决`数组/字符串`相关问题的算法思想，可以用来减少不必要的重复计算，从而提高效率，特别是在`处理连续数据`的场景下

`滑动窗口`通常涉及到一个窗口（可以是数组或字符串的一部分），这个窗口可以在数据结构上向前滑动
- 不断的调节子序列的起始位置和终止位置，从而得出要想的结果
- 窗口的大小可以是`固定`的也可以是`动态变化`的

优点：
- `高效`：通过避免重复计算，滑动窗口可以减少不必要的计算，提高效率
- `直观`：这种方法很直观，易于理解和实现

### 使用场景

`滑动窗口`算法适用于许多类型的问题，如：
- `固定大小的问题`：如求长度为 `K` 的`子数组/子字符串`的`最大或最小值`
- `可变大小的问题`：如找到`数组/字符串`中满足`特定条件`的`最小或最大长度的子数组/子字符串`

滑动窗口算法的时间复杂度通常是 `O(n)`，其中 `n` 是数组或字符串的长度

空间复杂度则取决于窗口的大小和维护窗口状态所需的额外空间

# 滑动窗口模板

字符串实际上就是`数组`，所以这里把输入设置成字符串，做题时根据具体题目自行变通即可

注意，用合适的数据结构记录窗口中的数据，根据具体场景变通：
- 想记录窗口中元素出现的次数，就用 `map`
- 想记录窗口中的元素和，就用 `int`

```java
public static void slidingWindow(String s) {
    Map<Character, Integer> window = new HashMap<>();
    int left = 0, right = 0;
    while (right < s.length()) {
        // c 是将移入窗口的字符
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        // ...
        // 判断左侧窗口是否要收缩
        while (left < right && window needs shrink) {
            // d 是将移出窗口的字符
            char d = s.charAt(left);
            window.put(d, window.get(d) - 1);
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            // ...
        }
    }
}
```
```python
def slidingWindow(s: str):
    window = dict()
    left = 0
    right = 0
    while right < len(s):
        # c 是将移入窗口的字符
        c = s[right]
        window[c] = window.get(c, 0) + 1
        # 增大窗口
        right += 1
        # 进行窗口内数据的一系列更新
        #...
        # 判断左侧窗口是否要收缩
        while left < right and "window needs shrink":
            # d 是将移出窗口的字符
            d = s[left]
            window[d] -= 1
            if window[d] == 0:
                del window[d]
            # 缩小窗口
            left += 1
            # 进行窗口内数据的一系列更新
            #...
```
```js
var slidingWindow = function(s) {
    var window = new Map();
    
    var left = 0, right = 0;
    while (right < s.length) {
        // c 是将移入窗口的字符
        var c = s[right];
        window.set(c, (window.get(c) || 0) + 1);
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        // 判断左侧窗口是否要收缩
        while (left < right && "window needs shrink condition") {
            // d 是将移出窗口的字符
            var d = s[left];
            var count = window.get(d);
            if(count == 1){
                window.delete(d);
            } else {
                window.set(d, count - 1);
            }
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}
```

这个滑动窗口框架也用了一个嵌套 `while` 循环，为什么复杂度是 `O(n)`？
- 指针 `left`, `right` 不会回退（它们的值只增不减），所以字符串/数组中的每个元素都只会进入窗口一次，然后被移出窗口一次，不会说有某些元素多次进入和离开窗口，所以算法的时间复杂度就和字符串/数组的长度成正比
- 反观嵌套 `for` 循环的暴力解法，`j` 会回退，所以某些元素会进入和离开窗口`多次`，所以时间复杂度就是 `O(n^2)`
