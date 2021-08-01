# 今天的题目是[剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

---

题目要求是从上到下、从左到右顺序打印二叉树，也就是按层遍历。
首先我们建立一个队列，然后队列放入根节点。
随后进入循环，判断条件为这个队列是否有元素，然后依次将该队列的第一个值弹出并保存在输出数组中；
之后添加子节点，判断该节点是否有左节点，有则加入队列；是否有右节点，有则加入队列，重复上述循环过程。
这里有个特例：当根节点为空的时候则返回空列表。

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
        res, deque = [], collections.deque([root])
        while deque:
            tmp = collections.deque()
            for _ in range(len(deque)):
                node = deque.popleft()
                if len(res) % 2: tmp.appendleft(node.val) 
                else: tmp.append(node.val) 
                if node.left: deque.append(node.left)
                if node.right: deque.append(node.right)
            res.append(list(tmp))
        return res
```

```c++
class Solution {
private:
    vector<vector<int> > ans;
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(root==nullptr)//特判
        return ans;
        vector<int> temp;
        queue<TreeNode*> Q;
        Q.emplace(root);
        bool lefttoright=true;
        while(!Q.empty())
        {
            for(int i=Q.size();i>0;--i)//每次由当前队列大小来遍历每层的树节点
            {
                TreeNode* front=Q.front();
                temp.emplace_back(front->val);
                Q.pop();
                if(front->left)
                Q.emplace(front->left);
                if(front->right)
                Q.emplace(front->right);
            }
            if(!lefttoright)
            reverse(temp.begin(),temp.end());
            lefttoright=!lefttoright;
            ans.emplace_back(temp);
            temp.clear();
        }
        return ans;
    }
};
```

