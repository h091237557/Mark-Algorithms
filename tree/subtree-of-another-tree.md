---
description: 注意、Amazon
---

# Subtree of Another Tree

[1165. Subtree of Another Tree](https://www.lintcode.com/problem/subtree-of-another-tree/?_from=ladder&&fromId=119)

#### 題目

Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.

Example Example 1:

```text
Given tree s:

     3
    / \
   4   5
  / \
 1   2
Given tree t:
   4 
  / \
 1   2
Return true, because t has the same structure and node values with a subtree of s.
```

Example 2:

```text
Given tree s:

     3
    / \
   4   5
  / \
 1   2
    /
   0
Given tree t:
   4
  / \
 1   2
Return false.
```

#### 解法

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
private:
    bool compare(TreeNode* s, TreeNode* t){
        if(s == NULL){
            return t == NULL;
        }
        if(t == NULL || s->val != t->val){
            return false;
        }
        return compare(s->left, t->left) && compare(s->right, t->right);
    }
public:
    /**
     * @param s: the s' root
     * @param t: the t' root
     * @return: whether tree t has exactly the same structure and node values with a subtree of s
     */
    bool isSubtree(TreeNode * s, TreeNode * t) {
        // Write your code here
        if(s == NULL){
            return t == NULL;
        }

        if(compare(s,t)){
            return true;
        }

        return isSubtree(s->left, t) || isSubtree(s->right, t);
    }
};
```

