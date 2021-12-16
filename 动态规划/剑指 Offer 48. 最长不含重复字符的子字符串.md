# 今天的题是[剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

示例 1:

> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:

> 输入: "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:

> 输入: "pwwkew"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
>      

---

我们使用哈希表来保存每个字符最后一次出现的位置，当遍历到s[j]的时候，可以通过访问哈希表dic[s[j]]获得最近的相同字符串的索引i。

结合动态规划：

- 状态定义：dp[j]表示以字符串s[j]结尾的最长不重复子字符串的长度

- 转移方程：固定右边界j，设与s[j]相同字符的最左临近的位置为i。也就是s[i]等于s[j]

  当i小于0，s[j]左边没有相同的字符，dp[j]=dp[j-1]+1

  当dp[j-1]<j-i,说明字符s[i]在子字符串dp[j-1]区间之外，所以dp[j]=dp[j-1]+1

  因为i<0时，由于dp[j-1]<=j恒成立，所以dp[j-1]<=j-i恒成立，所以以上两个可以合并

  当dp[j-1]>j-i,说明字符s[i]在子字符串dp[j-1]区间内，所以dp[j]的左边界由i决定，dp[j]=j-i

- 返回值： max(dp) ，即全局的 “最长不重复子字符串” 的长度。



结合滑动窗口：

- 使尾指针向末尾方向移动

- 如果尾指针指向的元素存在于哈希表中，头指针跳跃到重复字符的下一位

- 不断更新哈希表和窗口长度。

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        dic = {}
        res = tmp = 0
        for j in range(len(s)):
            i = dic.get(s[j], -1) 
            dic[s[j]] = j
            tmp = tmp + 1 if tmp < j - i else j - i 
            res = max(res, tmp) 
        return res
```

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<char, int> m;
        int ret = 0, l = 0, r = 0;
        while (r < s.size()) {
            if (m.find(s[r]) != m.end()) {
                l = max(l, m[s[r]] + 1); //如果直接l= m[s[r]] + 1，abba，b先更新l的位置，然后a又退回去了
            }
            m[s[r++]] = r;
            ret = max(r - l, ret);
        }
        return ret;
    }
};
```

