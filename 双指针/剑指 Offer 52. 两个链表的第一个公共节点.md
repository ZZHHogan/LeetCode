# 今天的题目是[1743. 从相邻元素对还原数组](https://leetcode-cn.com/problems/restore-the-array-from-adjacent-pairs/)

输入两个链表，找出它们的第一个公共节点。

如下面的两个链表**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点 c1 开始相交。

**示例 1：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

---

这里我们使用两种方法，哈希表法与双指针法

#### 方法一：哈希表法

我们可以用一个哈希表来存取每个节点，如果节点已经存在哈希表内，那么该节点则为重复的节点，即相交的节点。

具体过程为：

- 先遍历headA链表，并将headA当前节点存入哈希表中
- 然后遍历headB链表，判断当前节点是否在哈希表中
- 若不存在，则继续表headB链表
- 若存在，则返回该节点

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        unordered_set<ListNode*> h;
        ListNode* tmp = headA;
        while (tmp != nullptr) {
            h.insert(tmp);
            tmp = tmp->next;
        }
        tmp = headB;
        while (tmp != nullptr) {
            if (h.count(tmp)) {
                return tmp;
            }
            tmp = tmp->next;
        }
        return nullptr;
    }
};
```

#### 方法二：双指针

这里我们使用双指针，node1，node2分别指向原先的链表headA、headB，然后开始分别遍历。当node1遍历到headA末尾的时候，重新从headB头结点遍历；node2也是重复这个过程。

因两个链表的长度分别为L1+C、L2+C，其中，L1、L2为非公共部分的长度，C为公共部分的长度。node1走了L1+C步后，回到headB起点走L2步；node2走了L2+C步后，headA起点走L1步。那么当两个结点都走了L1+L2+C步的时候，就相遇了。

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        node1, node2 = headA, headB
        while node1 != node2:
            node1 = node1.next if node1 else headB
            node2 = node2.next if node2 else headA
        return node1
```

