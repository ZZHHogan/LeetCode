# 今天的题是[剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

为了让您更好地理解问题，以下面的二叉搜索树为例：

 ![img](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)



 

我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。

![img](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

---

我们知道：二叉搜索树的中序遍历为递增序列 。
而题目中提到将 二叉搜索树 转换成一个 “排序的循环双向链表” ，包含三个条件：

- 首先是对链表进行排序，节点应从小到大排序，因此我们这里使用 中序遍历 “由小到大”访问树的节点。
- 建立双向链表： 在构建相邻节点的引用关系时，设前驱节点 pre 和当前节点 cur ，
- 注意：不仅要建立 pre.right = cur ，也要建立 cur.left = pre 。
- 最后就是循环链表： 设链表头节点 head 和尾节点 tail ，则应构建 head.left = tail 和 tail.right = head 。

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
"""
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        def dfs(cur):
            if not cur: return
            dfs(cur.left) # 递归左子树
            if self.pre: # 修改节点引用
                self.pre.right, cur.left = cur, self.pre
            else: # 记录头节点
                self.head = cur
            self.pre = cur # 保存 cur
            dfs(cur.right) # 递归右子树
        
        if not root: return
        self.pre = None
        dfs(root)
        self.head.left, self.pre.right = self.pre, self.head
        return self.head
```

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        if(!root) return NULL;
        vector<Node*> inorder1;
        inorder(inorder1,root);
        for(int i = 0;i < inorder1.size()-1;i++)
        {
            inorder1[i]->right = inorder1[i+1];
        }
        for(int j = inorder1.size()-1;j>=1;j--)
        {
            inorder1[j]->left = inorder1[j-1];
        }
        inorder1[inorder1.size()-1]->right = inorder1[0];
        inorder1[0]->left = inorder1[inorder1.size()-1];
        Node* head = inorder1[0];
        return head;
    }
    void inorder(vector<Node*> &inorder1,Node* root)
    {
        if(root == NULL)  return ;
        inorder(inorder1,root->left);
        inorder1.push_back(root);
        inorder(inorder1,root->right);
    }
};
```

