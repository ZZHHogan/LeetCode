# 今天的题是[剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

**输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。**

---

和普通的回溯思想类似，不断进行回溯，每次回溯使用目标值减去当前值，并判断当下的目标值是否为

- 初始化： 结果列表 res ，路径列表 path  
- 返回值： 返回 res 即可。 
- 回溯函数的入参： 
  - 当前节点 root ，当前目标值 tar ，也可以为其他，具体你自己定。
- 终止条件： 若节点 root 为空，则直接返回。 
- 递推： 
  - 路径更新：将当前节点值 root.val 加入路径 path 
  - 目标值更新：tar = tar - root.val，即目标值 tar 从 sum 减至 0 
  - 3.路径记录：当前结点为叶节点 且路径和等于目标值也就是差值tar为0 ，则将此路径 path 加入 res 
  - 4.继续遍历： 递归左 、 右子节点
  - 5.路径恢复： 当不符合条件进行向上回溯前，需要将当前节点从路径 path 中删除，也就是path.pop() 

```python
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        res, path = [], []
        def recur(root, tar):
            if not root: return
            path.append(root.val)
            tar -= root.val
            if tar == 0 and not root.left and not root.right:
                res.append(list(path))
            recur(root.left, tar)
            recur(root.right, tar)
            path.pop()

        recur(root, sum)
        return res
```

```cpp
class Solution {
public:
    vector<vector<int>> ret;
    vector<int> path;

    void dfs(TreeNode* root, int target) {
        if (root == nullptr) {
            return;
        }
        path.emplace_back(root->val);
        target -= root->val;
        if (root->left == nullptr && root->right == nullptr && target == 0) {
            ret.emplace_back(path);
        }
        dfs(root->left, target);
        dfs(root->right, target);
        path.pop_back();
    }

    vector<vector<int>> pathSum(TreeNode* root, int target) {
        dfs(root, target);
        return ret;
    }
};
```

