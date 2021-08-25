# 今天的题是[剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)

---

我们知道二叉搜索树的特点：

若任意节点的左子树不空，则左子树上所有结点的值均小于它的根结点的值；

若任意节点的右子树不空，则右子树上所有结点的值均大于它的根结点的值；

并且任意左右子树都没有键值相等的节点。

基于此，所以可以首先判断p、q是否在root的左右子树，如果是的话那么就一定会出现（左边节点的值 - 根节点的值） * （右边节点的值 - 根节点的值）小于零；否则就会出现大于零的现象，因为作差之后两边等号。

然后就判断其中一个节点的值时大于根节点还是小于根节点的，大于则在右子树，小于则在左子树。然后就递归更改根节点，再重复操作即可。

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        return root
```

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while ((root->val - p->val) * (root->val - q->val) > 0) {
            if (root->val > p->val ) {
                root = root->left;
            } else {
                root = root->right;
            }
        }
        return root;
    }
};
```

