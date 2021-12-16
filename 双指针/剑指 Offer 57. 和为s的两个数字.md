# 今天的题是[剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

---

#### 哈希表法：

这道题可以用哈希表法，先以当前的数字为key，目标和s减去当前数字为value，然后遍历后面数组的同时判断有无存在目标和s减去当前数字的key存在，存在则返回当前值以及所包含的key。

#### 双指针法：

因为题目中提出该数组为排序数组，所以这里利用双指针从数组的两端开始出发，也叫对撞指针。

计算是否存在i、j下标下的值使两者和为s

如果大于目标和s则让右指针往左移动一位；如果小于目标和s则让左指针往右移动一位；如果等于目标和s则立刻返回数字组合。

如果两者碰撞之后还没找到答案，则返回空数组。

```python
class Solution(object):
    def twoSum(self, nums, target):
        if not nums or len(nums) == 1:
            return []
        dic = {}
        for i in nums:
            if (target - i) in dic:
                return [i, target-i]
            dic[i] = 0
        return []
```

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int l=0,r=nums.size()-1;
        while(l<r)
        {
            if(nums[l]+nums[r]>target)
            --r;
            else if(nums[l]+nums[r]<target)
            ++l;
            else
            return {nums[l],nums[r]};
        }
        return {};
    }
};
```

