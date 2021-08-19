# 今天的题是[剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

---

定义一组快慢指针，快指针先走k步，然后慢指针开始走。此时两个指针之间相隔k步。

当快指针遍历到链表末尾时，慢指针所在的位置就是倒数第K个节点。

需要注意判断的是：

- k是否大于链表长度，如果大于那么慢指针还没开始走，快指针就已经遍历到链表末端了
- 链表是否为空
- k是否小于0

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
        former, latter = head, head
        for _ in range(k):
            if not former: return
            former = former.next
        while former:
            former, latter = former.next, latter.next
        return latter
```

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* pre = head;
        ListNode* p = head;
        for (int i = 0; i < k; i++) {
            pre = pre->next;
        }
        while (pre != NULL) {
            p = p->next;
            pre = pre->next;
        }
        return p;
    }
};
```

