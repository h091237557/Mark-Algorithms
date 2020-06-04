---
description: Amazon
---

# Symmetric Binary Tree

[468. Symmetric Binary Tree](https://www.lintcode.com/problem/symmetric-binary-tree/?_from=ladder&&fromId=131)

#### 題目

Given a binary tree, check whether it is a mirror of itself \(i.e., symmetric around its center\).

Example Example 1:

Input: {1,2,2,3,4,4,3} Output: true Explanation: 1 /  2 2 /  /  3 4 4 3

is a symmetric binary tree.

Example 2:

```text
Input: {1,2,2,#,3,#,3}
Output: false
Explanation:
         1
        / \
        2  2
        \   \
         3   3

is not a symmetric binary tree.
```

#### 解法 - Recursively

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
    bool _isSymmetric(TreeNode* left, TreeNode* right){
        if(left == NULL && right == NULL){
            return true;
        }

        if(left == NULL || right == NULL){
            return false;
        }

        if(left->val != right->val){
            return false;
        }

        return _isSymmetric(left->left, right->right) && _isSymmetric(right->left, left->right);
    }
public:
    /**
     * @param root: the root of binary tree.
     * @return: true if it is a mirror of itself, or false.
     */
    bool isSymmetric(TreeNode * root) {
        if(root == NULL){
            return true;
        }
        // write your code here
        return _isSymmetric(root->left, root->right);
    }
};
```

#### 解法 - Iteratively

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
     * @param root: the root of binary tree.
     * @return: true if it is a mirror of itself, or false.
     */
    bool isSymmetric(TreeNode * root) {
        // write your code here
        if(root == NULL){
            return true;
        }

        std::queue<TreeNode*> queue;
        queue.push(root->left);
        queue.push(root->right);

        while(!queue.empty()){
            TreeNode* left = queue.front();
            queue.pop();
            TreeNode* right = queue.front();
            queue.pop();

            if(left == NULL && right == NULL){
                return true;
            }

            if(left == NULL || right == NULL){
                return false;
            }

            if(left->val != right->val){
                return false;
            }

            if(left->left){
                queue.push(left->left);
            }else{
                if(left->val != -1){
                    queue.push(new TreeNode(-1));
                }
            }

            if(right->right){
                queue.push(right->right);
            }else{
                if(right->val != -1){
                    queue.push(new TreeNode(-1));
                }
            }

            if(left->right){
                queue.push(left->right);
            }else{
                if(left->val != -1){
                    queue.push(new TreeNode(-1));
                }
            }

            if(right->left){
                queue.push(right->left);
            }else{
                if(right->val != -1){
                    queue.push(new TreeNode(-1));
                }
            }
        }
        return true;
    }
};
```

