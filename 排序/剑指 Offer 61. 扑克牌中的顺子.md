# 今天的题是[剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

---

一开始拿到这道题，找不到突破口，后来想想既然要组成顺子，那么这组牌应该是连续的并且最大值减去最小值为4.

但是题目又说有大小王，我们可以看做是万能牌，可以代替任意一张牌，所以最大值减去最小值应该小于等四。

注意的是，如果牌中有重复的数字，那么就不能构成顺子，可以提前返回。

因此，大致思路为：

1.除大小王外，所有牌 无重复 

2.除了大小王，这五张牌的最大值减去最小值满足：max−min<5 

我们使用哈希表来保存遍历过的牌，观察是否出现重复的数，并且在遍历的过程中保存最大值与最小值。

 复杂度分析： 

- 时间复杂度 O(N)： 其中 N 为nums长度，遍历数组使用 O(N）时间。 
- 空间复杂度 O(N)： 用于判重的哈希表， 使用)O(N) 额外空间。

```python
class Solution:
    def isStraight(self, nums: List[int]) -> bool:
        joker = 0
        nums.sort()
        for i in range(4):
            if nums[i] == 0: 
                joker += 1 
            elif nums[i] == nums[i + 1]: 
                return False 
        return nums[4] - nums[joker] < 5 
```

```cpp
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        vector<int> map(14);       
        int minValue = INT_MAX, maxValue = INT_MIN;
        for(int n : nums) 
        {
            if(map[n] >= 1) return false;   
            if(n == 0)  continue;           
            minValue = min(minValue, n);
            maxValue = max(maxValue, n);
            ++map[n];
        }
        return maxValue - minValue <= 4;    
    }
};
```

