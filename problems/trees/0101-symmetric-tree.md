# 101. Symmetric Tree

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/symmetric-tree/
- **Status:** solved
- **Last touched:** 2026-08-18

## Problem

Return whether a binary tree is a mirror image of itself around its center.

## Approach

A direct extension of [Same Tree](0100-same-tree.md). Same three-case structure, same lockstep
two-tree walk — the *only* change is which children get paired up.

- **Same Tree** asks for exact equality: `left->left` vs `right->left` (same side, both trees).
- **Symmetric Tree** asks for mirror equality: **outer with outer, inner with inner.**
  - outer: `left->left` vs `right->right`
  - inner: `left->right` vs `right->left`

That pairing rule is the conceptual heart of the problem, and it was derived unprompted this
session.

The wrapper is back, but doing a different job than in the traversal problems: `isSymmetric` has
the wrong *arity*, not the wrong parameters. It receives one root; the real question needs two
nodes. So it does no comparison of its own — it just seeds the first pair `(root->left,
root->right)` and hands off to the two-argument `isMirror`.

## Complexity

- Time: O(n)
- Space: O(h) recursion stack

## Solution

```cpp
class Solution {
public:
    bool isMirror(TreeNode* left, TreeNode* right){
        if(left==NULL && right==NULL) return true;
        if(left==NULL || right==NULL) return false;
        if(left->val != right->val) return false;
        return isMirror(left->left, right->right) && isMirror(left->right, right->left);
    }
    bool isSymmetric(TreeNode* root) {
        return isMirror(root->left, root->right);
    }
};
```

## Notes / mistakes

The confusion here was **syntax and scope, not concept** — the mirror rule itself came out right
the first time.

- **Referenced `root` inside `isMirror`.** That function's parameters are named `left` and
  `right`; there is no `root` in scope. A name only exists inside the function that declares it.
- **Wrote `isSymmetric(root->left) && isSymmetric(root->right)`** — calling the one-argument
  wrapper twice instead of the two-argument helper once. This would check each subtree against
  *itself*, never against each other, so it can't detect asymmetry between the two halves at all.

## Follow-up: the wrapper has no null guard

`isSymmetric` dereferences `root->left` without first checking `root`. This **passes on LeetCode**
because the problem constrains the tree to at least 1 node, so `root` is never null — but on an
empty tree it would crash.

Worth noticing given the null-guard ordering bug that cost several rounds on LC 100 the same
session: `isMirror` guards null carefully, and then the wrapper that calls it doesn't. Defensive
version:

```cpp
bool isSymmetric(TreeNode* root) {
    if (root == nullptr) return true;
    return isMirror(root->left, root->right);
}
```

Minor: this file uses `NULL` while [LC 100](0100-same-tree.md) uses `nullptr`. In C++ prefer
`nullptr` — it's typed as a pointer, whereas `NULL` is an integer constant, which is exactly the
ambiguity behind the `->val == NULL` bug logged in [BUGWATCH.md](../../BUGWATCH.md).
