# 今天的题目是[剑指 Offer 49. 丑数](https://leetcode-cn.com/problems/restore-the-array-from-adjacent-pairs/)

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

---

从题目中可以知道：

丑数的递推性质： 丑数只包含因子 2, 3, 5，因此有 “丑数 == 某较小丑数 * 某因子”
动态规划解析：
状态定义： 设动态规划列表 dp ，当前第i个元素dp[i]代表第 i + 1个丑数；
转移方程：每轮计算 dp[i]后，使其始终满足方程条件。
在每一轮计算中，需要更新索引 a, b, c的值，
分别独立判断dp[i] 和 dp[a]×2 , dp[b]×3 , dp[c]×5 的大小关系，若相等则将对应索引 a , b , c 加 1 ；
并且取三种情况的最小值作为新的丑数

dp[a]×2>dp[i−1]≥dp[a−1]×2
dp[b]×3>dp[i−1]≥dp[b−1]×3
dp[c]×5>dp[i−1]≥dp[c−1]×5
dp[i]=min(dp[a]×2,dp[b]×3,dp[c]×5)
初始状态： dp[0] = 1 ，即第一个丑数为 1 ；
返回值： dp[n-1] ，即返回第 n 个丑数

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        factors = [2, 3, 5]
        seen = {1}
        heap = [1]

        for i in range(n - 1):
            curr = heapq.heappop(heap)
            for factor in factors:
                if (nxt := curr * factor) not in seen:
                    seen.add(nxt)
                    heapq.heappush(heap, nxt)

        return heapq.heappop(heap)
```

```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        int a = 0, b = 0, c = 0;
        int dp[n];
        dp[0] = 1;
        for(int i = 1; i < n; i++) {
            int n2 = dp[a] * 2, n3 = dp[b] * 3, n5 = dp[c] * 5;
            dp[i] = min(min(n2, n3), n5);
            if(dp[i] == n2) a++;
            if(dp[i] == n3) b++;
            if(dp[i] == n5) c++;
        }
        return dp[n - 1];
    }
};
```

