# 628. Maximum Subtree

[628. Maximum Subtree](https://www.lintcode.com/problem/maximum-subtree/description?_from=ladder&&fromId=11)

#### 題目

Given an integer array nums, find the contiguous subarray \(containing at least one number\) which has the largest sum and return its sum.

```text
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

```text
Input:
{1,-5,2,0,3,-4,-5}
Output:3
Explanation:
The tree is look like this:
     1
   /   \
 -5     2
 / \   /  \
0   3 -4  -5
The sum of subtree 3 (only one node) is the maximum. So
```

Follow up:

If you have figured out the O\(n\) solution, try coding another solution using the divide and conquer approach, which is more subtle.

#### 小練習

**產生 Sum Tree**

```cpp
TreeNode* generateSumTree(TreeNode* root){
        if(root == NULL){
            return nullptr;
        }

        TreeNode* res = new TreeNode(0);
        res->left = generateSum(root->left);
        res->right = generateSum(root->right);

        res->val = root->val;
        if(res->left){
            res->val += res->left->val;
        }
        if(res->right){
            res->val += res->right->val;
        }
        return res;
    }
```

#### 解法 - 遞迴

使用遞迴來計算『 每個節點的最大總合值 』，並且再使用此值來比較『 現在最大總合值 』，如果大於他，則此節點設為答案。

由於每個節點都會跑到，因此如何跑都沒關係。

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
    int maxSum;
    TreeNode* res;
    int generateSubTreeSum(TreeNode* root){

        if(root == nullptr){
            return 0;
        }

        int leftSum = generateSubTreeSum(root->left);
        int rightSum = generateSubTreeSum(root->right);

        int rootSum = root->val + leftSum + rightSum;
        if(rootSum > maxSum){
            res = root;
            maxSum = rootSum;
        }
        return rootSum;
    }
public:
    /**
     * @param root: the root of binary tree
     * @return: the maximum weight node
     */
    TreeNode * findSubtree(TreeNode * root) {
        if(root == nullptr){
            return root;
        }

        maxSum = INT_MIN;
        generateSubTreeSum(root);
        return res;
    }
};
```

