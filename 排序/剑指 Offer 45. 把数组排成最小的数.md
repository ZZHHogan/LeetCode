# 今天的题是[剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

**输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。**

---

其实思路就是将数组中的每个数拿出来，然后相加、进行比较。只不过这里不是单纯地相加，而是需要转为字符串，然后拼接起来，再比较两者的结果。

1. 首先进行初始化，字符串列表 strs ，保存各数字的字符形式；
2.  对列表进行排序： 根据规则对 strs 执行排序： 
   - 若拼接字符串 x + y > y + x ，则 x “大于” y ；
   -  反之，若 x + y < y + x ，则 x “小于” y ； 
   - x “小于” y 代表：排序完成后，数组中 x 应在 y 左边；“大于” 则反之。 
3.  拼接 strs 中的所有字符串，并返回。

复杂度分析：

 时间复杂度 O(NlogN) ：N为将数字变为字符串strs后的数量，当使用快排的平均时间复杂度为O(NlogN) 。

 空间复杂度 O(N) ：N为字符串列表strs占用线性大小的额外空间。

```python
class Solution:
    def minNumber(self, nums: List[int]) -> str:
        def quick_sort(l , r):
            if l >= r: 
                return
            i, j = l, r
            while i < j:
                while strs[j] + strs[l] >= strs[l] + strs[j] and i < j: 
                    j -= 1
                while strs[i] + strs[l] <= strs[l] + strs[i] and i < j: 
                    i += 1
                strs[i], strs[j] = strs[j], strs[i]
            strs[i], strs[l] = strs[l], strs[i]
            quick_sort(l, i - 1)
            quick_sort(i + 1, r)
        
        strs = [str(num) for num in nums]
        quick_sort(0, len(strs) - 1)
        return ''.join(strs)
```

```cpp
class Solution {
public:
    string minNumber(vector<int>& nums) {
        string res;
        sort(nums.begin(), nums.end(), cmp);
        for (int &num : nums)
            res += to_string(num);             
        return res;
    }

    static bool cmp(int &x, int &y) {
        return to_string(x) + to_string(y) < to_string(y) + to_string(x);
    }
};
```

