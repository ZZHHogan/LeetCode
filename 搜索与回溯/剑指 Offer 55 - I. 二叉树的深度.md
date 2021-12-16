# 今天的题是[剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

---

- 深度优先搜索

终止条件： 当目前节点为空的时候，说明已经到叶节点了，这时返回深度为0 ；

然后依次计算节点的左子树的深度与节点右子树的深度。

记得返回的深度需要加一，因为即使是只有一层的根节点，它的高度是为1的。

- 广度优先搜索

使用队列形式的广度优先搜索，首先往队列加入根节点，每遍历一层，那么深度就加一，

与此同时判断当前节点是否有左子树或者右子树，有的话则加入队列，进行下一轮的遍历，最后返回深度值即可

```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        que = []
        depth = 0
        que.append(root)
        while que:
            tmp = []
            for node in que:
                if node.left:
                    tmp.append(node.left)
                if node.right:
                    tmp.append(node.right)
            que = tmp
            depth += 1
        return depth
```

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root)   return 0;
        return  max(maxDepth(root->left),maxDepth(root->right)) + 1;
    }
};
```

