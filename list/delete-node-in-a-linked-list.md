---
description: Microsoft
---

# Delete Node in a Linked List

[372. Delete Node in a Linked List](https://www.lintcode.com/problem/delete-node-in-a-linked-list/?_from=ladder&&fromId=21)

#### 題目

Implement an algorithm to delete a node in the middle of a singly linked list, given only access to that node.

Example Example 1:

```text
Input:
1->2->3->4->null
3
Output:
1->2->4->null
```

Example 2:

```text
Input:
1->3->5->null
3
Output:
1->5->null
```

#### 解法

```cpp
/**
 * Definition of ListNode
 * class ListNode {
 * public:
 *     int val;
 *     ListNode *next;
 *     ListNode(int val) {
 *         this->val = val;
 *         this->next = NULL;
 *     }
 * }
 */


class Solution {
public:
    /*
     * @param node: the node in the list should be deleted
     * @return: nothing
     */
    void deleteNode(ListNode * node) {
        // write your code here
        if(node == NULL){
            return;
        }

        node->val = node->next->val;
        node->next = node->next->next;
    }
};
```

