# 145. Binary Tree Postorder Traversal

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/binary-tree-postorder-traversal/
- **Status:** solved
- **Last touched:** 2026-08-18

## Problem

Return the values of a binary tree's nodes in postorder: entire left subtree, then entire right
subtree, then the node itself.

## Approach

Same wrapper/helper skeleton as [94](0094-binary-tree-inorder-traversal.md) and
[144](0144-binary-tree-preorder-traversal.md). The `push_back` moves to the **end**, after both
recursive calls. That completes the trio:

| Order | Shape | Root position |
| --- | --- | --- |
| Inorder | L, **Root**, R | middle |
| Preorder | **Root**, L, R | first |
| Postorder | L, R, **Root** | last |

The root is visited last at every level, so by the time a node is processed both of its children
have already been fully handled. That's the whole reason to pick postorder: it's the natural fit
whenever a node's own computation *depends on results from both children*.

- Tree height / size — need both subtree answers before combining
- Deleting or freeing a tree — children must go before the parent
- Postfix (Reverse Polish) expression evaluation — operands before operator

## Complexity

- Time: O(n)
- Space: O(n) output + O(h) recursion stack

## Solution

```cpp
class Solution {
public:
    void solve(TreeNode* node, vector<int>& result) {
        if (!node) return;
        solve(node->left, result);
        solve(node->right, result);
        result.push_back(node->val);
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        solve(root, result);
        return result;
    }
};
```

## Notes / mistakes

- None — written correctly first attempt, no syntax errors. The wrapper/helper pattern from
  inorder/preorder has stuck.
