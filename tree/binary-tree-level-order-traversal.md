# Binary Tree Level Order Traversal

[69. Binary Tree Level Order Traversal](https://www.lintcode.com/problem/binary-tree-level-order-traversal/?_from=ladder&&fromId=15)

#### 題目

Given a binary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

Example Example 1:

```text
Input：{1,2,3}
Output：[[1],[2,3]]
Explanation：
  1
 / \
2   3
it will be serialized {1,2,3}
level order traversal
```

Example 2:

```text
Input：{1,#,2,3}
Output：[[1],[2],[3]]
Explanation：
1
 \
  2
 /
3
it will be serialized {1,#,2,3}
level order traversal
```

Challenge Challenge 1: Using only 1 queue to implement it. Challenge 2: Use BFS algorithm to do it.

Notice

#### 解法 - BFS

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
     * @return: Level order a list of lists of integer
     */
    vector<vector<int>> levelOrder(TreeNode * root) {
        // write your code here
        vector<vector<int>> res;
        if(!root){
            return res;
        }

        std::queue<TreeNode*> queue;
        queue.push(root);

        while(!queue.empty()){
            int size = queue.size();
            vector<int> level;

            for (int i=0; i < size; i++){
                TreeNode* cur = queue.front();
                queue.pop();
                level.push_back(cur->val);

                if(cur->left){
                    queue.push(cur->left);
                }
                if(cur->right){
                    queue.push(cur->right);
                }
            }
            res.push_back(level);
        }
        return res;
    }
};
```

