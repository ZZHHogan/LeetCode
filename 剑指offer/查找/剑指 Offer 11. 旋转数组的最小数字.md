# 今天的题是[剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

```输入：[2,2,2,0,1]
输入：[3,4,5,1,2]
输出：1
```

```
输入：[2,2,2,0,1]
输出：0
```

---

初始化： 声明 i, j 双指针分别指向 nums 数组左右两端
循环二分： 设 m = (i + j) / 2为每次二分的中点（ "/" 代表向下取整除法，因此恒有i≤m<j ），**可分为以下三种情况：**

> 当 nums[m] > nums[j]时： m 一定在 左排序数组 中，即旋转点 x 一定在 [m + 1, j]闭区间内，因此执行 i = m + 1；
> 当 nums[m] < nums[j]时： m 一定在 右排序数组 中，即旋转点 x 一定在[i, m]闭区间内，因此执行 j = m；
> 当 nums[m] = nums[j]时： 因为有重复元素的存在，无法判断m 在哪个排序数组中，即无法判断旋转点 x 在[i,m] 还是 [m + 1, j区间中。

解决方案： 

执行 j = j - 1缩小判断范围，因为当nums[i]、nums[i+1]、nums[i+2]...nums[m]...nums[j]的值都一样的时候，旋转点肯定是在左侧而不是右侧的（以最左边的点进行旋转），所以这时候就可以缩小搜索空间。
返回值： 当 i = j时跳出二分循环，并返回旋转点的值 nums[i]即可。

```python
class Solution:
    def minArray(self, numbers: [int]) -> int:
        i, j = 0, len(numbers) - 1
        while i < j:
            m = (i + j) // 2
            if numbers[m] > numbers[j]: i = m + 1
            elif numbers[m] < numbers[j]: j = m
            else: j -= 1
        return numbers[i]
```

```cpp
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int i = 0, j = numbers.size() - 1;
        while (i < j) {
            int m = (i + j) / 2;
            if (numbers[m] > numbers[j]) i = m + 1;
            else if (numbers[m] < numbers[j]) j = m;
            else {
                int x = i;
                for(int k = i + 1; k < j; k++) {
                    if(numbers[k] < numbers[x]) x = k;
                }
                return numbers[x];
            }
        }
        return numbers[i];
    }
};
```

