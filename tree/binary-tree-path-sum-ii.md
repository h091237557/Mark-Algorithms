---
description: 注意、Amazon
---

# Binary Tree Path Sum II

[246. Binary Tree Path Sum II](https://www.lintcode.com/problem/binary-tree-path-sum-ii/?_from=ladder&&fromId=102)

#### 題目

Your are given a binary tree in which each node contains a value. Design an algorithm to get all paths which sum to a given value. The path does not need to start or end at the root or a leaf, but it must go in a straight line down.

Example Example 1:

```text
Input:
{1,2,3,4,#,2}
6
Output:
[[2, 4],[1, 3, 2]]
Explanation:
The binary tree is like this:
    1
   / \
  2   3
 /   /
4   2
for target 6, it is obvious 2 + 4 = 6 and 1 + 3 + 2 = 6.
```

Example 2:

```text
Input:
{1,2,3,4}
10
Output:
[]
Explanation:
The binary tree is like this:
    1
   / \
  2   3
 /   
4   
for target 10, there is no way to reach it.
```

#### 解法 - DFS

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
    void dfsHelper(
        TreeNode* root,
        vector<vector<int>>& res,
        vector<int>& sub,
        int target,
        int level
        ){

            if(!root){
                return;
            }

            sub.push_back(root->val);

            int temp = target;
            // 以此節點往上找和，看有沒有符合 target
            for (int i= level; i >= 0; i--){
                temp -= sub[i];
                // find a path of target
                if(temp == 0){
                    vector<int> subRes;
                    for (int j=i; j < sub.size(); j++){
                        subRes.push_back(sub[j]);
                    }
                    res.push_back(subRes);
                }
            }

            dfsHelper(root->left, res, sub, target, level+1);
            dfsHelper(root->right, res, sub, target, level+1);
            sub.pop_back();
    }
public:
    /*
     * @param root: the root of binary tree
     * @param target: An integer
     * @return: all valid paths
     */
    vector<vector<int>> binaryTreePathSum2(TreeNode * root, int target) {
        // write your code here
        vector<vector<int>> res;
        vector<int> sub;
        dfsHelper(root, res, sub, target, 0);
        return res;
    }
};
```

