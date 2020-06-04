# Reverse Linked List

[35. Reverse Linked List](https://www.lintcode.com/problem/reverse-linked-list/?_from=ladder&&fromId=15)

#### 題目

Reverse a linked list.

Example Example 1:

```text
Input: 1->2->3->null
Output: 3->2->1->null
```

Example 2:

```text
Input: 1->2->3->4->null
Output: 4->3->2->1->null
```

Challenge

Reverse it in-place and in one-pass

#### 解法

```cpp
/**
 * Definition of singly-linked-list:
 *
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
     * @param head: n
     * @return: The new head of reversed linked list.
     */
    ListNode * reverse(ListNode * head) {
        // write your code here
        ListNode* pre = nullptr;
        ListNode* next = nullptr;
        ListNode* cur = head;

        while(cur){
            next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
};
```

