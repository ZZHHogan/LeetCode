# 今天的题是[剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

---

和斐波那数列一样的思路：

- 斐波那契数列问题： f(0)=0 , f(1)=1 , f(2)=1 。
- 青蛙跳台阶问题： f(0)=1 , f(1)=1 , f(2)=2 ；

可以看出这都有着递推的性质：

- 当还剩一阶台阶的时候，有f(n-1)种跳法
- 当还剩两阶台阶的时候，有f(n-2)种跳法
- 所以f(n)=f(n−1)+f(n−2)

> 我们这里使用动态规划：
> 状态定义： 设使用一维数组作为dp数组，其中dp[i]代表有几种跳法
> 转移方程： 由上述分析：f(n)=f(n−1)+f(n−2)，得dp[i + 1] = dp[i] + dp[i - 1]
> 初始状态： dp[0]=1, dp[1] = 1
> 返回值： dp[n]返回最后一个数字

最后答案需要取模 1e9+7（1000000007），可能是防止溢出吧。

```python
class Solution:
    def numWays(self, n: int) -> int:
        if n <= 1:
            return 1
        if n == 2:
            return 2
        a, b = 1, 2
        for _ in range(3, n + 1):
            c = (a + b) % 1000000007
            a = b
            b = c
        return b
```

```c++
class Solution {
public:
    int numWays(int n) {
        if(n == 1 || n == 0) return 1;
        else if(n == 2) return 2; 
        int n1 = 1;
        int n2 = 2;
        for(int i = 3; i <= n; i++){
            auto n3 = (n1 + n2) % 1000000007;
            n1 = n2;
            n2 = n3;
        }
        return n2;
    }
};
```

