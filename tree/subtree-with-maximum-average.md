---
description: Amazon
---

# Subtree with Maximum Average

[597. Subtree with Maximum Average](https://www.lintcode.com/problem/subtree-with-maximum-average/?_from=ladder&&fromId=78)

#### 題目

Given a binary tree, find the subtree with maximum average. Return the root of the subtree.

Example Example 1

```text
Input：
{1,-5,11,1,2,4,-2}
Output：11
Explanation:
The tree is look like this:
     1
   /   \
 -5     11
 / \   /  \
1   2 4    -2 
The average of subtree of 11 is 4.3333, is the maximun.
```

Example 2

```text
Input：
{1,-5,11}
Output：11
Explanation:
     1
   /   \
 -5     11
The average of subtree of 1,-5,11 is 2.333,-5,11. So the subtree of 11 is the maximun.
```

Notice LintCode will print the subtree which root is your return node. It's guaranteed that there is only one subtree with maximum average.

#### 解法 - 分治法

這裡新選到一種 pair 的寫法，可以使用 {} 來產生。

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
    TreeNode* maxAvgNode;
    double maxAvg;
    pair<double,int> generateAvgAndNum(TreeNode* root){
        if(!root){
            return {0,0};
        }

        auto left = generateAvgAndNum(root->left);
        auto right = generateAvgAndNum(root->right);
        int num = left.second + right.second + 1;

        double avg = ((double)root->val + (left.first * left.second) + (right.first * right.second)) / num;
        if(avg > maxAvg){
            maxAvg = avg;
            maxAvgNode = root;
        }
        return { avg, num };
    }
public:
    /**
     * @param root: the root of binary tree
     * @return: the root of the maximum average of subtree
     */
    TreeNode * findSubtree2(TreeNode * root) {
        if(!root){
            return root;
        }

        maxAvg = INT_MIN;
        generateAvgAndNum(root);
        return maxAvgNode;
    }
};
```

