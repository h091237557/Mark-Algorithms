# Binary Tree Inorder Traversal

[67. Binary Tree Inorder Traversal](https://www.lintcode.com/problem/binary-tree-inorder-traversal/?_from=ladder&&fromId=21)

#### 題目

Given a binary tree, return the inorder traversal of its nodes' values.

Example Example 1:

```text
Input：{1,2,3}
Output：[2,1,3]
Explanation:
   1
  / \
 2   3
it will be serialized {1,2,3}
Inorder Traversal
```

Example 2:

```text
Input：{1,#,2,3}
Output：[1,3,2]
Explanation:
1
 \
  2
 /
3
it will be serialized {1,#,2,3}
Inorder Traversal
```

Challenge Can you do it without recursion?

#### 解法 - Recursion

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
    void traveral(TreeNode* root){
        if(root == NULL){
            return;
        }

        traveral(root->left);
        res.push_back(root->val);
        traveral(root->right);
    }
public:
    /**
     * @param root: A Tree
     * @return: Inorder in ArrayList which contains node values.
     */
    vector<int> inorderTraversal(TreeNode * root) {
        // write your code here
        traveral(root);
        return res;
    }
};
```

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
     * @param root: A Tree
     * @return: Inorder in ArrayList which contains node values.
     */
    vector<int> inorderTraversal(TreeNode * root) {
        // write your code here
        TreeNode* cur = root;
        vector<int> res;
        std::stack<TreeNode*> stack;

        while(cur != NULL || stack.size() != 0){
            while(cur != NULL){
                stack.push(cur);
                cur = cur->left;
            }

            cur = stack.top();
            stack.pop();
            res.push_back(cur->val);
            cur = cur->right;
        }
        return res;
    }
};
```

