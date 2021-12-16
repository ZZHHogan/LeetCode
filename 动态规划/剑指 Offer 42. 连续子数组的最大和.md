# 今天的题目是[剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

---

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

---

这题我们可以使用动态规划

- 动态规划每个数组元素的意义。首先定义动态规划数组的每一个元素代表什么，我们这里定义数组dp上的每一个元素都为以nums[i]结尾的连续子数组最大和

- 定义状态转移方程。如果dp[i-  1]小于0，那么说明dp[i-1]对dp[i]是产生负贡献的，也就是dp[i]+nums[i]比nums[i]本身小。因此，当dp[i-1]大于0的时候，dp[i] = dp[i - 1] + nums[i]；而当dp[i - 1]小于0的时候，我们就以nums[i]来代替当前的dp[i]，也就不要之前的dp[i-1]，起到一个重新计数的作用。

- 初始状态的定义，dp[0] = nums[0]，这里以第一个元素作为初始化。

- 所需要返回的结果。因为dp数组是动态地求取最优解，所以dp数组的最后一项即为我们所求，连续子数组的最大和

如果使用动态规划数组的方法，那么时间复杂度、空间复杂度都为O(n)。
这里我们可以使用两个数来保存当前的最优解以及上一过程的最优解，可以将空间复杂度降为O(1)。

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = nums[0];
        int tmp = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            tmp = max(nums[i], tmp + nums[i]);
            res = max(res, tmp);
        }
        return res;
    }
};
```



```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        res, tmp = nums[0], nums[0]
        for i in range(1, len(nums)):
            tmp = max(nums[i], tmp + nums[i])
            res = max(tmp, res)
        return res
```

