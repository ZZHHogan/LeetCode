# 今天的题是[剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

示例1：

> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4

---

我们知道两个链表都是递增的，所以可以通过遍历来交替判断大小。但是不知道第一个节点应该赋值给谁，因为不知道是l1链表头节点的值大还是l2链表头节点的值大，这里可以引入新的头节点，然后将每个链表的节点添加到新的节点之后。

因为链表是递增的，但是长度不一定相同。所以当其中一个遍历末尾的时候，就可以退出交替判断的循环，直接将剩余链表直接接在新链表后面。

最后返回新链表头节点的下一个节点（next）即可。

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(0)
        cur = dummy
        while l1 and l2:
            if l1.val <= l2.val:
                cur.next = l1
                l1 = l1.next
            else:
                cur.next = l2
                l2 = l2.next
            cur = cur.next

        cur.next = l1 if l1 else l2
        return dummy.next
```

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == NULL)
            return l2;
        if (l2 == NULL)
            return l1;
        ListNode* pHead = NULL;
        if (l1->val < l2->val) {
            pHead = l1;
            pHead->next = mergeTwoLists(l1->next, l2);
        }
        else {
            pHead = l2;
            pHead->next = mergeTwoLists(l1, l2->next);
        }
        return pHead;
    }
};
```

