# 100. Same Tree

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/same-tree/
- **Status:** solved
- **Last touched:** 2026-08-18

## Problem

Given the roots of two binary trees, return whether they are structurally identical and have the
same values at every node.

## Approach

A **new recursive shape**: instead of walking one tree and accumulating, this walks *two trees in
lockstep*, descending both by the same route at the same time.

No wrapper/helper split here — there's no accumulator to carry, so the function returns the
answer directly. That's the structural difference from the traversal problems.

The three-case structure is the thing to remember:

1. **Both null** → `true`. Two absent subtrees match.
2. **Exactly one null** → `false`. One tree ran out before the other; structures differ.
3. **Both non-null** → compare values, then recurse into the *same-position* children:
   `p->left` with `q->left`, `p->right` with `q->right`.

Order matters: the null guards must come **first**. Case 3 dereferences `p->val` and `q->val`, so
it's only safe once cases 1 and 2 have ruled out null.

## Complexity

- Time: O(n) — n = size of the smaller tree; short-circuits early on mismatch
- Space: O(h) recursion stack

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
};
```

## Notes / mistakes

Several rounds of debugging on this one — the concept was fine, the failures were ordering and
syntax.

- **Dereferenced `p->val` before null-checking `p`.** Wrote the "both non-null" case *above* the
  null guards, so the guards never got a chance to run. Null-pointer crash. The fix is ordering,
  not logic: guards first, always.
- **Wrote `p->val == NULL` instead of `p == nullptr`.** These ask completely different questions —
  the first asks "is this node's integer value zero", the second asks "does this node exist". It
  compiles, which is what makes it dangerous. Logged in [BUGWATCH.md](../../BUGWATCH.md) as its
  own pattern; distinct from the `node->value` member-name typo from LC 144.
- **Used `&` instead of `&&`.** Bitwise AND in place of logical AND. Recurred within the session.
- **Paired `p->left` with `q->right`** in the recursive call. "Same position" means same side of
  both trees — crossing them is the *mirror* rule, which is LC 101, not this problem.
- **Missing `return`** — a bare `false;` statement, which does nothing at all.
