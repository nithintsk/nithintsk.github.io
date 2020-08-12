---
title: Validate a Binary Search Tree
tags: [Trees, Leetcode, BST]
style: fill
color: secondary
description: Different methods to verify the validity of a binary search tree. This post assumes a basic understanding of what a binary tree is and goes on to explain how inorder traversal and recursion can be used to validate BSTs.
---

### LeetCode 98 - Validate a Binary Search Tree
-------
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

+ The left subtree of a node contains only nodes with keys **less than** the node's key.
+ The right subtree of a node contains only nodes with keys **greater than** the node's key.
+ Both the left and right subtrees must also be binary search trees.

#### **> In-Order Traversal - Concept of ascending order**

If we use in-order traversal to serialize a binary search tree, we can get a list of values in **ascending order**. Using *prev to keep a track of the last visited node, we can establish the relationship that the current node must be less than the previous node. But this comparison must only start once we reach the smallest node, i.e. the first NULL.

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        TreeNode* prev = NULL; 
        return validate(root, prev);
    }
    bool validate(TreeNode* node, TreeNode* &prev) {
        if (node == NULL) return true;
        if (!validate(node -> left, prev)) return false; // Traverse left subtree
        if (prev && prev -> val >= node -> val) return false; // Starts comparison from first non null prev.
        prev = node; // Update prev before moving to right subtree.
        return validate(node -> right, prev); 
    }
};
```

#### **> Standard Recursion - View the problem as leftsubtree and rightsubtree**

In this solution below, the logic to break out of the recursion is simple.
Notice how values being passed to minNode and maxNode parameter are changing.

To the left subtree, you pass the maxNode as the root, minNode does not change.
To the right subtree, you pass the minNode as the root, maxNode does not chage.

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return isValidBST(root, NULL, NULL);
    }

    bool isValidBST(TreeNode* root, TreeNode* minNode, TreeNode* maxNode) {
        if(!root) return true;
        if(minNode && root->val <= minNode->val || maxNode && root->val >= maxNode->val)
            return false;
        return isValidBST(root->left, minNode, root) && isValidBST(root->right, root, maxNode);
    }
};
```

&nbsp;
