---
description: Amazon
---

# Diameter of Binary Tree

[1181. Diameter of Binary Tree](https://www.lintcode.com/problem/diameter-of-binary-tree/?_from=ladder&&fromId=137)

#### 題目

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

Example Example 1:

```text
Given a binary tree 
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
```

Example 2:

```text
Input:[2,3,#,1]
Output:2

Explanation:
      2
    /
   3
 /
1
```

Notice The length of path between two nodes is represented by the number of edges between them.

#### 解法

!! update: 在 leetcode 用同樣解法會錯。

這題是要求一顆樹的最長直徑 Diameter，而它的公式為 :

> 左子樹的深度 + 右子樹的深度

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
    int generateMaxLengh(TreeNode* root){
        if(root == NULL){
            return 0;
        }
        return max(generateMaxLengh(root->left), generateMaxLengh(root->right)) + 1;
    }
public:
    /**
     * @param root: a root of binary tree
     * @return: return a integer
     */
    int diameterOfBinaryTree(TreeNode * root) {
        if(root == NULL){
            return 0;
        }
        // write your code here
        // left max path number + right max path number
        return generateMaxLengh(root->left) + generateMaxLengh(root->right);
    }
};
```

#### 正解

上面的答案，在 leetcode 中，會在此案例失敗。因為 diameter 不限制一定是根節點，也有可能是在某個子節點中。

```text
[4,-7,-3,null,null,-9,-3,9,-7,-4,null,6,null,-6,-6,null,null,0,6,5,null,9,null,null,-1,-4,null,null,null,-2]
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    int getMaxDepth(TreeNode* root){
        if(root == NULL){
            return 0;
        }

        int left = getMaxDepth(root->left);
        int right = getMaxDepth(root->right);
        return max(left, right) + 1;
    }   
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if(root == NULL){
            return 0;
        }

        int left = getMaxDepth(root->left);
        int right = getMaxDepth(root->right);
        return max(left+right, max(diameterOfBinaryTree(root->left), diameterOfBinaryTree(root->right)));
    }
};
```

