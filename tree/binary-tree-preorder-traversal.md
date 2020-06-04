---
description: Microsoft
---

# Binary Tree Preorder Traversal

[66. Binary Tree Preorder Traversal](https://www.lintcode.com/problem/binary-tree-preorder-traversal/?_from=ladder&&fromId=116)

#### 題目

Given a binary tree, return the preorder traversal of its nodes' values.

Example Example 1:

```text
Input：{1,2,3}
Output：[1,2,3]
Explanation:
   1
  / \
 2   3
it will be serialized {1,2,3}
Preorder traversal
```

Example 2:

```text
Input：{1,#,2,3}
Output：[1,2,3]
Explanation:
1
 \
  2
 /
3
it will be serialized {1,#,2,3}
Preorder traversal
```

Challenge Can you do it without recursion?

Notice

* The first data is the root node, followed by the value of the left and right son nodes, and "\#" indicates that there is no child node.
* The number of nodes does not exceed 20.

#### 解法 - Recusive

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
    vector<int> res;
    void traversal(TreeNode* root){
        if(root == NULL){
            return;
        }

        res.push_back(root->val);
        traversal(root->left); 
        traversal(root->right); 
    }
public:
    /**
     * @param root: A Tree
     * @return: Preorder in ArrayList which contains node values.
     */
    vector<int> preorderTraversal(TreeNode * root) {
        // write your code here
        traversal(root);
        return res;
    }
};
```

#### Non-Rescurive

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
     * @param root: A Tree
     * @return: Preorder in ArrayList which contains node values.
     */
    vector<int> preorderTraversal(TreeNode * root) {
        // write your code here
        vector<int> res;
        if(root == NULL){
            return res;
        }

        std::stack<TreeNode*> stack;
        stack.push(root);

        while(!stack.empty()){
            TreeNode* cur = stack.top();
            stack.pop();
            res.push_back(cur->val);

            if(cur->right){
                stack.push(cur->right);
            }

            if(cur->left){
                stack.push(cur->left);
            }
        }

        return res;
    }
};
```

