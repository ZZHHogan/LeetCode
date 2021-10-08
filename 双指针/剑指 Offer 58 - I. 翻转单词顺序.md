# 今天的题是[剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

---

我们使用两个指针left、right来帮助选择每一个单词

在每次循环时，先去除所有单词右侧空格，获取某个单词的最右下标right，再获取单词的最左下标left

然后把从left到right的单词加入到结果ret中，之后再加一个空格

最后记得要把最后一次添加的空格去掉

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s.strip()
        s_list = s.split()
        s_list.reverse()
        return " ".join(s_list)
```

```cpp
class Solution {
public:
    string reverseWords(string s) {
        int n = s.size(), l, r = n - 1;
        string ret;
        while(r >= 0){  
            while(r >= 0 && s[r] == ' ') --r; 
            if(r < 0) break;
            for(l = r; l >= 0 && s[l] != ' '; --l); 
            ret += (s.substr(l + 1, r - l) + " ");
            r = l;
        }
        if(ret.size()) ret.pop_back();
        return ret;
    }
};
```

