# 今天的题目是[剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

---

这里注意需要**每一层打印到一行**

题目要求和剑指 Offer 32 - I. 从上到下打印二叉树差不多，不同的是，同一层的要打印到一行中。这里要求是按照从上到下、从左到右顺序打印二叉树，也就是按层遍历。
首先我们建立一个队列，然后队列放入根节点。
随后进入循环，判断条件为这个队列是否有元素，然后依次将该队列的第一个值弹出并保存在输出数组中；
注意的是，这里需要做一个循环保存当前这一层的节点，循环次数为当前层节点数（即队列 queue 长度）。
在循环过程中也需要添加子节点，判断该节点是否有左节点，有则加入队列；是否有右节点，有则加入队列，重复上述循环过程。这里有个特例：当根节点为空的时候则返回空列表。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root: return []
        res, queue = [], collections.deque()
        queue.append(root)
        while queue:
            tmp = []
            for _ in range(len(queue)):
                node = queue.popleft()
                tmp.append(node.val)
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
            res.append(tmp)
        return res
```

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) 
    {
        if (!root)  return {};
        vector<vector<int>> res;    // 结果数组
        queue<TreeNode*> que;       // 辅助队列
        que.push(root);             
        while (!que.empty())
        {
            int cnt = que.size();   // 当前层的节点数
            vector<int> level;      // 保存当前层的元素
            // 处理当前层的元素，每个元素出队前将其左右子节点入队，作为下一层的元素
            while (cnt)
            {
                auto node = que.front();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
                level.push_back(node->val);
                que.pop();
                cnt--;
            }
            res.push_back(level);   // 得到了一层的元素
        }
        return res;
    }
};
```

