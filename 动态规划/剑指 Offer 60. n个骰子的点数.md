# 今天的题是[剑指 Offer 60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

> 示例 1:
>
> 输入: 1
> 输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]

> 示例 2:
>
> 输入: 2
> 输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]

---

首先进行初始化，我们知道，当只有一个骰子的时候，每个数字出现的概率都为1/6，当骰子个数增加时，总数也随之增加，从2开始递增。
当新加骰子后的点数组合，如：(1,1), (1,2), ⋯, (6,6)，共有6 的n次方种。
因此，新数组tmp存放点数和的范围，范围为[n, 6n]，数量为 6n - n + 1 = 5n + 1种。
当进入for j in range(len(dp))时，表示骰子数为i-1时，所有骰子总数和出现的情况，j值表示数组中的索引，并不表示骰子总数和的数值，
for k in range(6)表示：新加骰子可能出现的点数1-6,在数组中影响前面索引加0-5的索引指向的值（范围指向1-6）。
这里进行正向递推，此时的dp表示骰子总数为i-1的数组出现的每一个点数和，之后从k中每次单独拿出来依次和新点数(1~6)相加，计算相加后点数和的概率 ，最后，骰子总数为i的数组tmp赋值骰子总数为i-1的数组dp

```python
class Solution:
    def dicesProbability(self, n: int) -> List[float]:
        dp = [1 / 6] * 6
        for i in range(2, n + 1):
            tmp = [0] * (5 * i + 1)
            for j in range(len(dp)):
                for k in range(6):
                    tmp[j + k] += dp[j] / 6
            dp = tmp
        return dp
```

```cpp
class Solution {
public:
    vector<double> dicesProbability(int n) {
        vector<double> dp(6, 1.0 / 6.0);
        for (int i = 2; i <= n; i++) {
            vector<double> tmp(5 * i + 1, 0);
            for (int j = 0; j < dp.size(); j++) {
                for (int k = 0; k < 6; k++) {
                    tmp[j + k] += dp[j] / 6.0;
                }
            }
            dp = tmp;
        }
        return dp;
    }
};
```

