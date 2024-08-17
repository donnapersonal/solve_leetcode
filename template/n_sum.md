# 一个方法解决 nSum 问题

`nSum` 类型问题的通用解法，支持 `2sum`、`3sum`、`4sum`... 等

`n == 2` 是 `twoSum` 的`双指针`解法，`n > 2` 时就是`穷举`第一个数字，然后`递归`调用计算 `(n-1)Sum`，组装答案

> 注意：调用这个 `nSum` 函数之前一定要先给 `nums` 数组排序
> - 因为 `nSum` 是一个递归函数，若在 `nSum` 函数里调用排序函数，则每次递归都会进行没有必要的排序，导致效率非常低

时间复杂度分析：
- `n = 2` 时，时间复杂度 `O(nlogn)`，排序所消耗的时间
- `n > 2` 时，时间复杂度为 `O(n^n-1)`，即 `n` 的 `n-1` 次方，至少是 `2` 次方，此时可省略排序所消耗的时间，如：`3sum` 为 `O(n^2)`，`4sum` 为 `O(n^3)`...

```java
class Solution {
    public List<List<Integer>> nSumTarget(int[] nums, int n, int start, int target) {
        int len = nums.length;
        List<List<Integer>> res = new ArrayList<>();
        // 至少是 2Sum，且数组大小不应该小于 n
        if (n < 2 || len < n) return res;
        // 2Sum 是 base case
        if (n == 2) {
            int left = start, right = len - 1;
            while(left < right) {
                int sum = nums[left] + nums[right];
                int left = nums[left], right = nums[right]
                if(sum < target) {
                    while(left < right && nums[left] == left) left++;
                } else if(sum > target) {
                    while(left < right && nums[right] == right) right--;
                } else {
                    res.add(Arrays.asList(nums[left], nums[right]));
                    while (left < right && nums[left] == left) left++;
                    while (left < right && nums[right] == right) right--;
                }
            }
        } else {
            // n > 2 时，递归计算 (n-1)Sum 的结果
            for (int i = start; i < len; i++) {
                List<List<Integer>> sub = nSumTarget(nums, n - 1, i + 1, target - nums[i]);
                for (List<Integer> arr : sub) {
                    // (n-1)Sum 加上 nums[i] 就是 nSum
                    arr.add(nums[i]);
                    res.add(arr);
                }
                while (i < len - 1 && nums[i] == nums[i + 1]) i++;
            }
        }
        return res;
    }
}
```
```python
def nSumTarget(nums, n, start, target):
    numslen = len(nums)
    res = []
    # 至少是 2Sum，且数组大小不应该小于 n
    if n < 2 or numslen < n:
        return res
    # 2Sum 是 base case
    if n == 2:
        # 双指针那一套操作
        left, right = start, numslen - 1
        while left < right:
            numsum = nums[left] + nums[right]
            l, r = nums[left], nums[right]
            if numsum < target:
                while left < right and nums[left] == l:
                    left += 1
            elif numsum > target:
                while left < right and nums[right] == r:
                    right -= 1
            else:
                res.append([left, right])
                while left < right and nums[left] == l:
                    left += 1
                while left < right and nums[right] == r:
                    right -= 1
    else:
        # n > 2 时，递归计算 (n-1)Sum 的结果
        for i in range(start, numslen):
            sub = nSumTarget(nums, n - 1, i + 1, target - nums[i])
            for arr in sub:
                # (n-1)Sum 加上 nums[i] 就是 nSum
                arr.append(nums[i])
                res.append(arr)
            while i < numslen - 1 and nums[i] == nums[i + 1]:
                i += 1
    
    return res
                    
# 示例使用
nums = [1, 0, -1, 0, -2, 2]
target = 0
nums.sort()
result = nSumTarget(nums, 4, 0, target)
for r in result:
    print(r)
```
```js
var nSumTarget = function(nums, n, start, target) {
    let res = [];
    let len = nums.length;
    // 至少是 2Sum，且数组大小不应该小于 n
    if(n < 2 || len < n) return res;
    // 2Sum
    if(n == 2) {
        // res = towSumTarget(nums, start, target);
        let left = start, right = len - 1; // 双指针那一套操作
        while (left < right) {
            let sum = nums[l] + nums[r];
            let l = nums[left], r = nums[right];
            if (sum < target) {
                while (left < right && nums[left] == l) left++;
            } else if (sum > target) {
                while (left < right && nums[right] == r) right--;
            } else {
                res.push([l, r]);
                while (left < right && nums[left] == l) left++;
                while (left < right && nums[right] == r) right--;
            }
        }
    } else {
        for (let i = start; i < len; i++) {
            // 递归求(n - 1)sum
            let subRes = nSumTarget(nums, n - 1, i + 1, target - nums[i]);
            for (let j = 0; j < subRes.length; j++) {
                res.push([nums[i], ...subRes[j]]);
            }
            // 跳过相同元素
            while (nums[i] === nums[i + 1]) i++;
        }
    }
    return res;
};
```