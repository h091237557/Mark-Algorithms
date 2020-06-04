---
description: Microsoft
---

# Flatten Binary Tree to Linked List

[453. Flatten Binary Tree to Linked List](https://www.lintcode.com/problem/flatten-binary-tree-to-linked-list/?_from=ladder&&fromId=21)

#### 題目

Flatten a binary tree to a fake "linked list" in pre-order traversal.

Here we use the right pointer in TreeNode as the next pointer in ListNode.

Example Example 1:

```text
Input:{1,2,5,3,4,#,6}
Output：{1,#,2,#,3,#,4,#,5,#,6}
Explanation：
     1
    / \
   2   5
  / \   \
 3   4   6

1
\
 2
  \
   3
    \
     4
      \
       5
        \
         6
```

Example 2:

```text
Input:{1}
Output:{1}
Explanation：
         1
         1
```

Challenge Do it in-place without any extra memory.

#### 解法 - Non-Recursion

```cpp
/**
 * Definition of TreeNode:
 * class TreeNode {
 * public:
 *     int val;
 *     TreeNode *left, *right;
 *     TreeNode(int val) {
 *         this->val = val;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param root: a TreeNode, the root of the binary tree
     * @return: nothing
     */
    void flatten(TreeNode * root) {
        // write your code here

        while(root){
            if(root->left){
                TreeNode* pre = root->left;
                while(pre->right){
                    pre = pre->right;
                }

                pre->right = root->right;
                root->right = root->left;
                root->left = NULL;
            }
            root = root->right;
        }
    }
};
```

#### 解法 - Non-Recursion Stack

