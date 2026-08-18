# 144. Binary Tree Preorder Traversal

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/binary-tree-preorder-traversal/
- **Status:** solved
- **Last touched:** 2026-08-18

## Problem

Return the values of a binary tree's nodes in preorder: node first, then its entire left subtree,
then its entire right subtree.

## Approach

Identical skeleton to [inorder](0094-binary-tree-inorder-traversal.md) — same wrapper/helper
split, same `vector<int>&` accumulator, same null base case. The **only** change is where
`push_back` sits: at the top of the function instead of between the two recursive calls.

The root is processed the instant it's reached, before descending into either subtree. That
"root position" is the entire difference between the three orders.

## Complexity

- Time: O(n)
- Space: O(n) output + O(h) recursion stack

## Solution

```cpp
class Solution {
public:
    void solve(TreeNode* node, vector<int>& result) {
        if (!node) return;
        result.push_back(node->val);
        solve(node->left, result);
        solve(node->right, result);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        solve(root, result);
        return result;
    }
};
```

## Trace

Same tree `[1,2,3,4,5]`. Preorder = `1, 2, 4, 5, 3`.

Compare inorder on the same tree: `4, 2, 5, 1, 3`.

## When preorder is the right choice

- **Cloning / rebuilding a tree** — you need the root in hand before you can attach children to it.
- **Serialization** — write the root first so the reader can reconstruct top-down.
- **Prefix (Polish) expression notation** — operator before operands.

Contrast: inorder's special property is sorted output on a BST; preorder has no such ordering
guarantee, it's about having the parent available first.

## Notes / mistakes

- Wrote `node->value` instead of `node->val`. The `TreeNode` struct member is `val` — caught it by
  going back and rereading the struct definition rather than guessing again. Logged in
  [BUGWATCH.md](../../BUGWATCH.md) as a pattern: inventing a plausible-sounding member name under
  time pressure.
