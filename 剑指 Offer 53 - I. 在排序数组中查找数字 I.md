# 今天的题是在排序数组中查找数字 I

统计一个数字在排序数组中出现的次数。

**示例 1:**

> 输入: nums = [5,7,7,8,8,10], target = 8
> 输出: 2

**示例 2:**

> 输入: nums = [5,7,7,8,8,10], target = 6
> 输出: 0

---

因为题目所给的数组是排序数组，所以我们这里可以使用二分法的思想来解决。

不过因为要找的是一个区间，因此我们找到区间的左边界与右边界，左右边界这区间里的值都为target。

第一，我们初始化： 左边界 i = 0，右边界 j = len(nums) - 1。然后开始计算中间点，用的是常规的二分法循环求解。

若 nums[mid] < target ，则 target 在闭区间 [mid + 1, j]中，因此执行 i = mid + 1；
若 nums[mid] > target ，则 target在闭区间 [i, mid - 1]中，因此执行 j = mid - 1；
若 nums[mid] = target，则右边界 right 在闭区间 [mid + 1, j] 中；以及左边界 left 在闭区间 [i, mid - 1]中。因此分为以下两种情况：

若查找 右边界 right ，则需要执行 i = m + 1 ；（跳出时 i指向右边界）
若查找 左边界 left ，则需要执行 j = m - 1 ；（跳出时 j 指向左边界）

我们最终使用了两次二分查找，分别查找左右边界right 和 left的值，最终返回 right - left - 1就可以得到我们的答案。

进行优化：
以下优化基于：

- 查找完右边界 right = i 后，则 nums[j]指向最右边的 target （若存在）。

- 查找完右边界后，可用 nums[j] = j 判断数组中是否包含 target ，若不包含则直接提前返回 0 ，无需后续查找左边界。
- 查找完右边界后，左边界 left 一定在闭区间 [0, j]中，因此直接从此区间开始二分查找即可。

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.size() == 0) {
            return 0;
        }
        //初始左右指针位置
        int i = 0;
        int j = nums.size()-1;
        //第一次二分：找right边界
        //这边是“小于等于”，因此当循环结束后，ij不重合，且如果存在target值的话，
		//i的位置就是右边界（target值序列右边第一个大于target值的位置），因为最后一次循环一定是i=mid+1；且此时j指向target
        while(i <= j) {
            int mid = (i+j) >> 1;
            //这里是“小于等于”，目的是为了确定右边界，就是说当mid等于target时，
			//因为不确定后面还有没有target，所以同样需要左边收缩范围
            if(nums[mid] <= target){
                i = mid+1;
            }
            else{
                j = mid-1;
            }
        }
        //在更新right边界值之前，需要判断这个数组中是否存在target，
		//如果不存在（看j指向的位置是不是target，为什么看的是j指针？详见上面的注释）
        if(j>=0&&nums[j] != target){
            return 0;
        }
        int right = i;    //更新right边界
        //重置指针
        i = 0;
        j = nums.size()-1;
        //第二次二分：找left边界
        //结束之后，j指向target序列左边第一个小于它的位置，i指向target（经过上面判断，target一定存在）
        while(i <= j) {
            int mid = (i+j) >> 1;
            //这里是“大于等于”，目的是确定左边界，因为就算当mid等于target的时候，
			//因为不确定左边还有没有target，所以同样需要收缩右边界
            if(nums[mid] >= target){
                j = mid-1;
            }
            else{
                i= mid+1;
            }
        }
        //更新左指针
        int left = j;
        return right-left-1;
    }
};
```

```python
class Solution:
    def search(self, nums: [int], target: int) -> int:
        # 搜索右边界 right
        i, j = 0, len(nums) - 1
        while i <= j:
            m = (i + j) // 2
            if nums[m] <= target: i = m + 1
            else: j = m - 1
        right = i
        # 若数组中无 target ，则提前返回
        if j >= 0 and nums[j] != target: return 0
        # 搜索左边界 left
        i = 0
        while i <= j:
            m = (i + j) // 2
            if nums[m] < target: i = m + 1
            else: j = m - 1
        left = j
        return right - left - 1
```

