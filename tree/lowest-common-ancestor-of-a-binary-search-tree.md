---
description: 注意、Microsoft
---

# Lowest Common Ancestor of a Binary Search Tree

[1311. Lowest Common Ancestor of a Binary Search Tree](https://www.lintcode.com/problem/lowest-common-ancestor-of-a-binary-search-tree/?_from=ladder&&fromId=104)

#### 題目

Given a binary search tree \(BST\), find the lowest common ancestor \(LCA\) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants \(where we allow a node to be a descendant of itself\).”

Given binary search tree: root = {6,2,8,0,4,7,9,\#,\#,3,5}  
  
[https://media-cdn.jiuzhang.com/markdown/images/7/6/660f4a1a-9ff0-11e9-a529-0242ac110002.jpg](https://media-cdn.jiuzhang.com/markdown/images/7/6/660f4a1a-9ff0-11e9-a529-0242ac110002.jpg)

Example Example 1:

```text
Input: 
{6,2,8,0,4,7,9,#,#,3,5}
2
8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

Example 2:

```text
Input: 
{6,2,8,0,4,7,9,#,#,3,5}
2
4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

* All of the nodes' values will be unique.
* p and q are different and both values will exist in the BST.

#### 解法

* 如果一個節點是左子樹，而另一個節點是在右子樹。則他們的 LCA 一定是根節點。
* 如果兩個都是在左子樹或右子樹，那 LCA 一定是他們中比較高的。

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
     * @param root: root of the tree
     * @param p: the node p
     * @param q: the node q
     * @return: find the LCA of p and q
     */
    TreeNode * lowestCommonAncestor(TreeNode * root, TreeNode * p, TreeNode * q) {
        // write your code here
        if(p->val > root->val && q->val > root->val){
            // 往右子樹找
            return lowestCommonAncestor(root->right, p, q);
        }

        if(p->val < root->val && q->val < root->val){
            // 往左子樹找
            return lowestCommonAncestor(root->left, p, q);
        }

        return root;
    }
};
```

