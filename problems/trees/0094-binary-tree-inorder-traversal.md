# 94. Binary Tree Inorder Traversal

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/binary-tree-inorder-traversal/
- **Status:** solved
- **Last touched:** 2026-08-18

## Problem

Return the values of a binary tree's nodes in inorder: for any node, visit its entire left
subtree, then the node itself, then its entire right subtree.

## Approach

Straight recursion on the definition. The only wrinkle is plumbing:

- **Wrapper/helper split.** LeetCode fixes the signature of `inorderTraversal` — it takes only
  `root`, so there's nowhere to hang the accumulator. A separate `solve` helper takes the extra
  `vector<int>&` parameter. `inorderTraversal` declares the one true `result` vector, hands it to
  `solve`, and returns it once the recursion has fully unwound.
- **Accumulator by reference.** `vector<int>&`, not `vector<int>`. Every recursive call gets its
  own stack frame, so a by-value parameter would give each call a private copy that dies when the
  call returns — nothing would ever accumulate into a single shared answer. The reference makes
  all frames write into the same vector. (See open question below.)

Base case is the null node: return immediately, no push.

## Complexity

- Time: O(n) — each node visited once
- Space: O(n) for the output, plus O(h) recursion stack (h = height; O(n) worst case on a skewed tree)

## Solution

```cpp
class Solution {
public:
    void solve(TreeNode* node, vector<int>& result) {
        if (!node) return;
        solve(node->left, result);
        result.push_back(node->val);
        solve(node->right, result);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        solve(root, result);
        return result;
    }
};
```

## Trace

Tree `[1,2,3,4,5]` — root 1, children 2 and 3; 2's children are 4 and 5.

Inorder = `4, 2, 5, 1, 3`. Traced by hand and confirmed call-by-call against the code.

## Notes / mistakes

- On a BST, inorder is the traversal that comes out **sorted**. That property is specific to
  inorder and is the usual reason to reach for it over the other two orders.

## Open questions — resolved

- ~~Why `vector<int>&` and not `vector<int>`~~ — **closed 2026-08-18** on
  [LC 543](0543-diameter-of-binary-tree.md). A reference aliases the caller's memory; a value
  parameter is a per-frame copy discarded on return. Confirmed against `swap(int&, int&)`.
