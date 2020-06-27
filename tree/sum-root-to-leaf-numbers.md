---
description: jane30
---

# Sum Root to Leaf Numbers

[129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

#### 題目

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1-&gt;2-&gt;3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

Note: A leaf is a node with no children.

```text
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```

```text
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

#### 解法 - DFS

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
private:
    int res = 0;
    void dfsHelper(TreeNode* node, int sum){
        if(node == NULL){
            return;
        }

        int total = (sum*10 + node->val);
        if(!node->left && !node->right){
            res += total;
            return;
        }

        dfsHelper(node->left, total);
        dfsHelper(node->right, total);
    }
public:
    int sumNumbers(TreeNode* root) {
        dfsHelper(root, 0);
        return res;
    }
};
```

#### 解法 - BFS

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        if(root == NULL){
            return 0;
        }

        int res = 0;
        std::queue<pair<TreeNode*, int>> queue;
        queue.push({root, 0});

        while(!queue.empty()){
            int size = queue.size();

            while(size != 0){
                size--;
                pair<TreeNode*, int> cur = queue.front();
                queue.pop();
                TreeNode* node = cur.first;
                int sum = cur.second*10 + node->val;

                if(!node->left && !node->right){
                    res += sum;
                }

                if(node->left){
                    queue.push({node->left, sum});
                }
                if(node->right){
                    queue.push({node->right, sum});
                }
            }
        }
        return res;
    }
};
```

