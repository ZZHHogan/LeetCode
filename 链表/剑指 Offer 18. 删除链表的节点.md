# 今天的题是[剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

---

首先我们判断需要删除的节点是否为原链表的头节点，如果是的话则无法直接删除。

所以我们新建一个链表，来保存原链表，所以即使原来链表的头节点为空，新链表cur的初始值也不为空。

当找到需要删除的节点后，进行常规删除操作，又因为只需要删除一个节点，所以我们删除了一个之后就可以退出循环了。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def deleteNode(self, head: ListNode, val: int) -> ListNode:
        if head.val == val: return head.next
        pre, cur = head, head.next
        while cur and cur.val != val:
            pre, cur = cur, cur.next
        if cur: pre.next = cur.next
        return head
```

```cpp
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        ListNode* dummy=new ListNode(-1,head);
        ListNode* cur=dummy;
        while(cur->next)
        {
            if(cur->next->val==val)
            {
                cur->next=cur->next->next;
                break;
            }
            cur=cur->next;
        }
        return dummy->next;
    }
};
```

