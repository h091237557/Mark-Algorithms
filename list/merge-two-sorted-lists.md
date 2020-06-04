---
description: Amazon
---

# Merge Two Sorted Lists

[165. Merge Two Sorted Lists](https://www.lintcode.com/problem/merge-two-sorted-lists/?_from=ladder&&fromId=15)

#### 題目

Merge two sorted \(ascending\) linked lists and return it as a new sorted list. The new sorted list should be made by splicing together the nodes of the two lists and sorted in ascending order.

Example

```text
Example 1:
    Input: list1 = null, list2 = 0->3->3->null
    Output: 0->3->3->null


Example 2:
    Input:  list1 =  1->3->8->11->15->null, list2 = 2->null
    Output: 1->2->3->8->11->15->null
```

#### 解法

就是把這兩合在一起。

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
     * @param l1: ListNode l1 is the head of the linked list
     * @param l2: ListNode l2 is the head of the linked list
     * @return: ListNode head of linked list
     */
    ListNode * mergeTwoLists(ListNode * l1, ListNode * l2) {
        // write your code here
        if(l1 == nullptr){
            return l2;
        }
        if(l2 == nullptr){
            return l1;
        }


        ListNode* dummy = new ListNode(-1);
        ListNode* temp = dummy;

        while(l1 != nullptr || l2 != nullptr){
            if(l1 == nullptr && l2 != nullptr){
                temp->next = new ListNode(l2->val);
                l2 = l2->next;
                temp = temp->next;
                continue;
            }

            if(l2 == nullptr && l1 != nullptr){
                temp->next = new ListNode(l1->val);
                l1 = l1->next;
                temp = temp->next;
                continue;
            }

            if(l1->val <= l2->val){
                temp->next = new ListNode(l1->val);
                l1 = l1->next;
            }else{
                temp->next = new ListNode(l2->val);
                l2 = l2->next;
            }
            temp = temp->next;
        }

        return dummy->next;
    }
};
```

