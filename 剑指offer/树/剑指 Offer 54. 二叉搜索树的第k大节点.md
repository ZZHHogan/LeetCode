# 今天的题是[剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

给定一棵二叉搜索树，请找出其中第k大的节点。

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```

---

二叉搜索树的特点：当进行中序遍历的时候，所有节点是从小到大排序的 

题目说的是第k大节点，并不是第k小节点，所以我们进行中序遍历的逆序就好，因此，本题的遍历顺序是：右 -> 中 -> 左。

开始递归遍历时计数，统计当前节点的序号。这里使用的是成员变量 k 来统计节点的序号 

当递归到第 k 个节点时，当 k == 0 时，则表示获取到了第 k 个节点，此时便可直接返回，并记录输出的结果res。

当找到我们想要的结果后，我们就可以停止遍历，返回了。因为递归函数时void，所以返回的时候为空，只能不断结束递归，递归到出口后再进行返回，而不能找到答案后立即返回。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def kthLargest(self, root: TreeNode, k: int) -> int:
        def dfs(root):
            if not root: return
            dfs(root.right)
            if self.k == 0: return
            self.k -= 1
            if self.k == 0: self.res = root.val
            dfs(root.left)

        self.k = k
        dfs(root)
        return self.res
```

```cpp
class Solution {
public:
    int a;
    int kthLargest(TreeNode* root, int k) {
        dfs(root, k);
        return a;
    }
    void dfs(TreeNode* root, int& k)
    {
        if(!root) return;
        dfs(root->right, k);
        k--;
        if(!k) a = root->val;
        if(k > 0) dfs(root->left, k);
    }
};
```

