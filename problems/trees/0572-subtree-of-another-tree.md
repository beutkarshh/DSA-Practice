# 572. Subtree of Another Tree

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/subtree-of-another-tree/
- **Status:** solved
- **Last touched:** 2026-08-22

## Problem

Return whether `subRoot` appears as a subtree of `root` — that is, whether some node of `root`,
together with all its descendants, is identical to `subRoot`.

## Approach

**First problem in the track built out of a previously solved one.** `isSameTree` from
[LC 100](0100-same-tree.md) is pasted in unchanged and used as a primitive; the new work is only
the outer search that decides *where* to apply it.

Two separate recursions, doing different jobs:

- `isSameTree(p, q)` — walks two trees in lockstep asking "are these identical?"
- `isSubtree(root, subRoot)` — walks one tree asking "does a match start anywhere in here?"

### The OR chain

`subRoot` is somewhere in `root` if **any** of three things holds:

1. `isSameTree(root, subRoot)` — it matches right here, starting from this node
2. `isSubtree(root->left, subRoot)` — it's somewhere in the left subtree
3. `isSubtree(root->right, subRoot)` — it's somewhere in the right subtree

`isSubtree` calling itself on the children is the same move as the height family: ask the
*identical* question of a smaller piece rather than writing fresh search logic.

### The base case is not the same as isSameTree's

Easy to get backwards. Here `root == NULL` returns **false**, not true.

The two functions are asking different questions, so their empty cases differ. `isSameTree`'s
"both null -> true" means *two absent subtrees match*. `isSubtree`'s "root is null -> false" means
*ran out of tree to search without finding anything*. `subRoot` is guaranteed non-null by the
constraints, so an empty `root` can't possibly contain it.

### Return the expression, not a branch on it

A boolean OR already evaluates to `true`/`false`:

```cpp
return A || B || C;                          // this
if (A || B || C) return true; else return false;   // not this
```

The second form also tempts you into evaluating `A` twice, which is what happened here — see below.

## Complexity

- Time: O(m x n) worst case. `isSameTree` may run at every node of `root` (m nodes), and each call
  costs up to O(n) where n = nodes in `subRoot`.
- Space: O(h) recursion stack, h = height of `root`

## Solution

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == nullptr && q == nullptr) return true;
        if (p == nullptr || q == nullptr) return false;
        if (p->val != q->val) return false;
        return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }

    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        if (root == NULL) return false;
        return isSameTree(root, subRoot) || isSubtree(root->left, subRoot) || isSubtree(root->right, subRoot);
    }
};
```

## Notes / mistakes

- **Called `isSameTree(root, subRoot)` twice**, once per `if` branch, before consolidating into the
  single three-way OR. Same "don't recompute a subproblem you already have" lesson as binary
  exponentiation and as the stored `leftHeight`/`rightHeight` in
  [Diameter](0543-diameter-of-binary-tree.md) — but this time at the *whole-function* level rather
  than inside one function.
- **Wrote the same null-check condition twice** (`&&` in both lines) instead of `&&` then `||` for
  the "exactly one null" case. Classic copy-paste-without-editing. **Caught by diffing against the
  already-filed [0100 write-up](0100-same-tree.md)** — the tracker paid for itself here.
- **Case-sensitivity slips**, three times: `P` vs `p`, and `subroot` vs `subRoot` twice.

## Possible follow-up

The O(m x n) bound is the naive one. Serialising both trees and running a string search gets it to
O(m + n) — worth revisiting if string algorithms come up later.
