# 今天的题是[剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

---

定义dp[i]表示保存截止到当前的位置，有多少种组合方式

状态转移:如果当前字符串表示的值在[10,25]区间，则dp[i]=dp[i-1]+dp[i-2]

否则，等于dp[i-1]

初始化dp[0]=dp[1]=1，分别代表无数字和第一位数字

因为当第一第二位组成的数字在[10,25]区间，则应该有两种组合方式

所以dp[2]=dp[1]+dp[0]=2，又因为dp[1]=2，所以dp[0]为1

使用两个变量保存中间过程，以降低空间复杂度

最后返回最后一个值，表示组合的方式

```python
class Solution:
    def translateNum(self, num: int) -> int:
        s = str(num)
        a = b = 1
        for i in range(2, len(s) + 1):
            tmp = s[i - 2:i]
            c = a + b if "10" <= tmp <= "25" else a
            b = a
            a = c
        return a
```

