---
description: 注意、Amazon
---

# Path Sum III

[1240. Path Sum III](https://www.lintcode.com/problem/path-sum-iii/?_from=ladder&&fromId=126)

#### 題目

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards \(traveling only from parent nodes to child nodes\).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

Example Example 1:

```text
Input：root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
Output：3
Explanation：
      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

Example 2:

```text
Input：root = [11,6,-3], sum = 17
Output：1
Explanation：
      11
     /  \
    6   -3
Return 17. The path that sum to 17 is:

1.  11 -> 6
```

#### 解法 - 遞迴

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
    int findPath(TreeNode* root, int sum){
        if(root == NULL){
            return 0;
        }

        int res = 0;
        if(root->val == sum){
            res += 1;
        }

        res += findPath(root->left, sum - root->val);
        res += findPath(root->right, sum - root->val);
        return res;
    }
public:
    /**
     * @param root: 
     * @param sum: 
     * @return: the num of sum path
     */
    int pathSum(TreeNode * root, int sum) {
        // write your code here
        if(root == NULL){
            return 0;
        }
        return findPath(root, sum) + pathSum(root->left, sum) + pathSum(root->right, sum);
    }
};
```

