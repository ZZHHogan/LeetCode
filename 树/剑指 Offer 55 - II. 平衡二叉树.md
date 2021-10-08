# 今天的题是[剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

---

深度递归遍历，基本思路就是使用一个递归的函数来获取树的深度，

然后不断地比较左右子树的深度差是否小于等于1，等于1则继续遍历直到遍历到叶子结点。

如果大于1则代表不平衡，直接返回-1代表该树不平衡，

最后判断返回结果是否为-1代表是否为平衡二叉树，

注意的是，获取到深度的时候要加1，因为即使是只有一层的根节点，它的高度是为1的。

```python
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        def dfs(node):
            if not node:
                return 0
            left = dfs(node.left)
            if left == -1:
                return -1
            right = dfs(node.right)
            if right == -1:
                return -1
            return max(right, left) + 1 if abs(left - right) <= 1 else -1
        return dfs(root) != -1
```

```cpp
class Solution {
public:
    bool ret = true;
    int dfs(TreeNode* root) {
        if(!root || !ret) 
            return 0;
        int l = dfs(root->left) + 1,
            r = dfs(root->right) + 1;
        if(abs(l - r) > 1) 
            ret = false;
        return max(l, r);
    }

    bool isBalanced(TreeNode* root) {
        if(!root) 
            return true;
        return abs(dfs(root->left) - dfs(root->right)) <= 1 && ret;
    }
};
```

