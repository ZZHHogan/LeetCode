# 今天的题是[剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)

---

对二叉树进行深度遍历，直到root遇到p或者q或者遇到叶子节点，遇到后则会终止递归。

遇到叶子节点返回null，遇到p或者q返回root

对左子树进行递归操作，返回值为left，同理对右子树进行递归操作，返回值为right

根据left和right的值进行判断：

- 如果left、right同时为空，说明root的左右子树都不包括p、q，返回null
- 如果left、right同时不为空，则说明p、q分布在root的两侧，因此root为最近的公共祖先，返回root
- 如果left、right两者其一为空，另一不为空，说明p、q在不为空的那一侧，比如说left为空，right不为空，那么right一侧的根节点就是最近的公共节点，返回即可。

注意如果left、right同时为空也就是可以通过if not left和if not right进行判断，因为两者都为空的了，所以返回的值也为空，

```python
class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        if not root or root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left:
            return right
        if not right:
            return left
        return root
```

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root || root == p || root == q) return root;
        TreeNode* l = lowestCommonAncestor(root->left, p, q),
                * r = lowestCommonAncestor(root->right, p, q);
        
        if(l && r) return root;
        else if(l) return l;
        else if(r) return r;
        else return nullptr; 
    }
};
```

