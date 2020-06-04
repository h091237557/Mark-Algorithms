---
description: Microsoft
---

# Remove Nth Node From End of List

[174. Remove Nth Node From End of List](https://www.lintcode.com/problem/remove-nth-node-from-end-of-list/?_from=ladder&&fromId=116)

#### 題目

Given a linked list, remove the nth node from the end of list and return its head.

Example

```text
Example 1:
    Input: list = 1->2->3->4->5->null， n = 2
    Output: 1->2->3->5->null


Example 2:
    Input:  list = 5->4->3->2->1->null, n = 2
    Output: 5->4->3->1->null
```

Challenge Can you do it without getting the length of the linked list?

Notice The minimum number of nodes in list is n.

#### 解法

1. 先讓 cur 往前走到 n 步。
2. 然後在讓 prev 與 cur 一起一步一步走到 cur 為 null。
3. 最後 prev 的位置就是要移除的前一個位置。

```cpp
/**
 * Definition of singly-linked-list:
 * class ListNode {
 * public:
 *     int val;
 *     ListNode *next;
 *     ListNode(int val) {
 *        this->val = val;
 *        this->next = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param head: The first node of linked list.
     * @param n: An integer
     * @return: The head of linked list.
     */
    ListNode * removeNthFromEnd(ListNode * head, int n) {
        // write your code here

        ListNode* res = new ListNode(-1);
        res->next = head;
        ListNode* prev = res;
        ListNode* cur = head;

        for (int i=0; i < n; i++){
            cur = cur->next;
        }

        while(cur != NULL){
            prev = prev->next;
            cur = cur->next;
        }

        prev->next = prev->next->next;

        return res->next;
    }
};
```

