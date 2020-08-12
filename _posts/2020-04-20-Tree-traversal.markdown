---
title: "Tree Traversal - Inorder, Preorder and Postorder"
tags: [Tree, traversal, inorder, preorder, postorder]
style: fill
color: success
description: A one stop reference to find the recursive and iterative traversal algorithms for different tree traversal approaches.
---

### Index
-------
1. [Representation of a Node in the Tree](#representation)
2. [Inorder Traversal - Recursive Method](#inorder_recursive)
3. [Inorder Traversal - Iterative Method](#inorder_iterative)
4. [Preorder Traversal](#preorder_recursive)
5. [Preorder Traversal](#preorder_iterative)
6. [Postorder Traversal](#postorder_recursive)
7. [Postorder Traversal](#postorder_iterative)
8. [Levelorder Traversal](#levelorder_traversal)

&nbsp;

### <a name="representation"></a> Representation of a node object in the tree
-------
Every Node object will hold an integer value, a pointer to its left child and a pointer to its right child and a constructor.

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

&nbsp;

### <a name="inorder_recursive"></a> Inorder Traversal - Recursive
-------
Inorder traversal - Print left child, Print node, Print right child

```cpp
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == NULL) return result;
        recursive(root, result);
        return result;
    }
    
    void recursive(TreeNode* root, vector<int> &result) {
        if(root == NULL) return;
        recursive(root -> left, result);
        result.push_back(root -> val);
        recursive(root -> right, result);
        return;
    }
```

&nbsp;

### <a name="inorder_iterative"></a> Inorder Traversal - Iterative (Using a stack)
-------
The stack is the perfect datastructure for use in this case as it can store a record of the chain of nodes visited.
The idea is to traverse as far left as possible while continuously adding nodes to the stack. Once NULL is reached, get the last inserted node in the stack, add it to the result and make the current node, the right child. This occurrs as long as the stack is not empty. We need `while(root || !stk.empty)` because initially, the stack is empty but root is not NULL. Similarly, when the left subtree completes, the stack will be empty but root will not be NULL.

```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> result;
    stack<TreeNode*> stk;
    while(root || !stk.empty()) {
        while(root) {
            stk.push(root);
            root = root->left;
        }
        root = stk.top();
        result.push_back(root->val);
        stk.pop();
        root = root->right;
    }
    return result;
}
```

&nbsp;

### <a name="preorder_recursive"></a> Preorder Traversal - Recursive
-------
Preorder traversal - Print node, Print left child, Print right child

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> result;
    preorder(root, result);
    return result;
}

void preorder(TreeNode* root, vector<int> &result) {
    if (root == NULL) return;
    result.push_back(root->val);
    preorder(root->left, result);
    preorder(root->right, result);
}
```

&nbsp;

### <a name="preorder_iterative"></a> Preorder Traversal - Iterative (Using a stack)
-------
This time around we use a stack to store the right child as we encounter it, while traversing the left subtree,
When we reach the end(NULL), pop the top of the stack, this will start traversing the right subtree of the last visited node.

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> result;
    stack<TreeNode*> stk;
    
    while (root || !stk.empty()) {
        while (root) {
            result.push_back(root -> val);
            stk.push(root -> right);
            root = root -> left;
        }
        root = stk.top();
        stk.pop();
    }
    
    return result;
}
```

### <a name="levelorder_traversal"></a> LevelOrder Traversal (Using a queue)
-------
Level Order Traversal means printing all the elements from left to right from the top level to the bottom.
The datastructure most suitable to accomplish this is a queue since it is FIFO.

The size of the queue is noted. This is the current number of nodes in the queue. Then, for each node at the front of the queue, children of the nodes are pushed into the queue, from left to right. The level has been traversed when `size` number of elements' children have been pushed in.

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector< vector<int> > result;
    queue<TreeNode*> Q;

    if (root == NULL) return result;
    Q.push(root);

    while (!Q.empty()) {
        int size = Q.size();
        vector<int> res;
        for (int i = 0; i < size; i++) {
            TreeNode* t = Q.front();
            Q.pop();
            res.push_back(t -> val);
            if (t -> left) Q.push(t -> left);
            if (t -> right) Q.push(t -> right);
        }
        result.push_back(res);
    }

    return result;
}
```


... T.B.C
