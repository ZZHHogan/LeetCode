# 今天的题是[剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

---

这道题使用滑动窗口，滑动窗口内部为从l到r的序列，并且我们可以使用等差公式来求窗口中所有数的总和sums，来与target比较

**等差公式：sums = (r - l + 1) * (l + r) / 2**

当左边界小于右边界的时候，进入循环，枚举所有可能的值，当左边界大于右边界的时候，就退出循环。

如果sums等于target，就将窗口内的序列加入结果中，

如果sums大于target，我们就收缩窗口，我们知道窗口内部的数字都为正数，因此当左边界向右移的时候，总和肯定是减小的，这时总和sums就往target靠拢；

同理，当sums小于target时，右边界右移，左边界不动，这时相当于新加了一个值进来，使sums变大。

因为滑动窗口左右边界都是向右移动的，所以这里的时间复杂度为O(n)

```python
class Solution:
    def findContinuousSequence(self, target: int) -> List[List[int]]:
        res = []
        
        l = 1
        r = 2
        while l < r:
            tmp = []
            sums = (r - l + 1) * (l + r) / 2
            if sums == target:
                for i in range(l, r + 1):
                    tmp.append(i)
                res.append(tmp[:])
                l += 1
            elif sums < target:
                r += 1
            else:
                l += 1
        return res
```

```cpp
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>>vec;
        vector<int> res;
        for (int l = 1, r = 2; l < r;){
            int sum = (l + r) * (r - l + 1) / 2;
            if (sum == target) {
                res.clear();
                for (int i = l; i <= r; ++i) {
                    res.emplace_back(i);
                }
                vec.emplace_back(res);
                l++;
            } else if (sum < target) {
                r++;
            } else {
                l++;
            }
        }
        return vec;
    }
};cpp代码块
```

