# Add Two Numbers

[167. Add Two Numbers](https://www.lintcode.com/problem/add-two-numbers/?_from=ladder&&fromId=15)

#### 題目

You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.

Example Example 1:

```text
Input: 7->1->6->null, 5->9->2->null
Output: 2->1->9->null    
Explanation: 617 + 295 = 912, 912 to list:  2->1->9->null
```

Example 2:

```text
Input:  3->1->5->null, 5->9->2->null
Output: 8->0->8->null    
Explanation: 513 + 295 = 808, 808 to list: 8->0->8->null
```

#### 解法

這一題比較記得要處理的 corner case 為如下這種類型。

```text
9->9
9
```

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
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
    ListNode * addLists(ListNode * l1, ListNode * l2) {
        // write your code here

        ListNode* dummy = new ListNode(-1);
        ListNode* temp = dummy;

        int carray = 0;
        int unit = 10;
        while (l1 != nullptr || l2 != nullptr){
            int sum = 0;
            if(l1 != nullptr){
                sum += l1->val;
                l1 = l1->next;
            }

            if(l2 != nullptr){
                sum += l2->val;
                l2 = l2->next;
            }
            sum += carray;

            if(sum >= unit){
                sum = sum - unit;
                temp->next = new ListNode(sum);
                carray = 1;
            }else{
                temp->next = new ListNode(sum);
                carray = 0;
            }

            temp = temp->next;
        }

        // 用來處理上述要注意的類型
        if(carray != 0){
            temp->next = new ListNode(1);
        }

        return dummy->next;
    }
};
```

