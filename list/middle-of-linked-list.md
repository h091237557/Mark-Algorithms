---
description: Mircosoft
---

# Middle of Linked List

[228. Middle of Linked List](https://www.lintcode.com/problem/middle-of-linked-list/?_from=ladder&&fromId=115)

#### 題目

Find the middle node of a linked list.

Example Example 1:

```text
Input:  1->2->3
Output: 2    
Explanation: return the value of the middle node.
```

Example 2:

```text
Input:  1->2
Output: 1    
Explanation: If the length of list is  even return the value of center left one.
```

Challenge If the linked list is in a data stream, can you find the middle without iterating the linked list again?

#### 解法

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
     * @param head: the head of linked list.
     * @return: a middle node of the linked list
     */
    ListNode * middleNode(ListNode * head) {
        // write your code here
        if(head == NULL){
            return NULL;
        }
        ListNode* slow = head;
        ListNode* fast = head->next;

        while(fast != nullptr && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }
};
```

