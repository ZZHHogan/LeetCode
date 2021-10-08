# 今天的题是[剑指 Offer 49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

---

由题可知，丑数的定义为：只包含质因子2、3、5的数，因此我们可以当做丑数为2的倍数或3的倍数、5的倍数 。

初始化：因为丑数的第一个元素为1，所以让数组第一个元素为1。 

我们以三个索引two、three、five来表示2的最小倍数所在的位置、3的最小倍数所在的位置、5的最小倍数所在的位置，

初始化都为首元素所在的索引：0.，然后通过比较当前数组中2的最小倍数再乘以2、3的最小倍数再乘以3、5的最小倍数再乘以5，

以此获取三者之中的最小值，并添加到新的元素中， 每次添加新元素之后，更新two、three、five所在的索引，

因为此时该值的最小倍数已经出现，需要找下一个最小倍数 。

注意这里是if、if、if而不是if else，因为如果是2、3的公共倍数6的话，那么2、3都需要更新索引 然后依次循环更新，

最终返回数组的最后一个值即可。 

时间复杂度 O(N)) ： 其中 N = n，为遍历的次数：n。

空间复杂度 O(N)： 长度为 N 的 输出数组使用 O(N)的额外空间。

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        two = 0
        three = 0
        five = 0
        res = [1]
        for i in range(1, n):
            min_num = min(res[two] * 2, res[three] * 3, res[five] * 5)
            res.append(min_num)
            if min_num % 2 == 0:
                two += 1
            if min_num % 3 == 0:
                three += 1
            if min_num % 5 == 0:
                five += 1
        return res[-1]
```

```cpp
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

