# 今天的题是[剑指 Offer 31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

---

因为栈先入后出的特点，所以某些弹出序列无法实现。这时借用一个模拟栈，入栈操作按照压栈序列，然后每次入栈之后循环判断栈顶的元素是否等于弹出序列

的当前元素，注意这里的栈顶元素索引为-1。

如果等于的话就将模拟栈的栈顶元素弹出，并且弹出序列的索引加一，最后判断模拟栈是否为空，为空则说明该弹出序列可以弹出压栈序列的各个元素，

若最后不为空，则说明弹出序列非法。

时间复杂度 O(N) ： 其中 N 为列表 pushed 的长度；每个元素最多入栈与出栈一次，即最多共 2N 次出入栈操作。

空间复杂度 O(N) ： 辅助栈 stack 最多同时存储 N 个元素。

```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        stack = []
        index = 0
        for num in pushed:
            stack.append(num)
            while len(stack) > 0 and stack[-1] == popped[index]:
                stack.pop()
                index += 1
        return len(stack) == 0
```

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        if(pushed.size() != popped.size()) return false;
        stack<int> stk;
        int i = 0;
        for(auto x : pushed){
            stk.push(x);
            while(!stk.empty() && stk.top() == popped[i]){
                stk.pop();
                i++;
            }
        }
        return stk.empty();
    }
};
```

