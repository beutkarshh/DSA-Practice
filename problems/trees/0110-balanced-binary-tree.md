# 110. Balanced Binary Tree

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/balanced-binary-tree/
- **Status:** solved
- **Last touched:** 2026-08-22

## Problem

Return whether a binary tree is height-balanced: every node's two subtrees differ in height by at
most 1.

## Approach

Same skeleton as [Diameter](0543-diameter-of-binary-tree.md) — a dual-purpose helper that
**returns** a height while **updating a tracker by reference** as a side effect. The difference is
what the tracker holds: Diameter carried a running max, this carries a bool verdict.

### The check must run at every node

A node is balanced when `abs(leftHeight - rightHeight) <= 1`. But the *tree* is balanced only if
that holds at **every** node — checking the root's two subtree heights is not enough, since a
deeply lopsided node further down leaves the root's own heights looking fine.

That's exactly why this rides along inside the height recursion: height already visits every node,
so the check gets applied everywhere for free. Anything else means recomputing heights per node
and paying O(n^2).

### Innocent until proven imbalanced

The tracker starts `true` and is only ever flipped to `false` — never back. One bad node anywhere
condemns the whole tree permanently. There's no need to short-circuit; later nodes can't undo the
verdict.

### Assignment, not return

The trap that cost the most time this session. The helper has **one return type**, `int`, and one
job for it: report height upward so the parent can continue its own computation. The balance
verdict is a *different* piece of information with a *different* lifetime, so it travels by
reference instead.

```cpp
if (abs(leftHeight - rightHeight) > 1) isBalanced = false;  // assignment
return 1 + max(leftHeight, rightHeight);                    // always the height
```

Returning early on imbalance would starve the parent of the height it needs. The height is
returned **regardless of the verdict**.

## Complexity

- Time: O(n) — each node visited once
- Space: O(h) recursion stack

## Solution

```cpp
class Solution {
public:
    int height(TreeNode* node, bool &isBalanced) {
        if (node == NULL) return 0;

        int leftHeight = height(node->left, isBalanced);
        int rightHeight = height(node->right, isBalanced);

        if (abs(leftHeight - rightHeight) > 1) isBalanced = false;

        return 1 + max(leftHeight, rightHeight);
    }

    bool isBalanced(TreeNode* root) {
        bool balanced = true;
        height(root, balanced);
        return balanced;
    }
};
```

As in Diameter, the wrapper discards `height`'s return value — it only wants the side effect.

## Notes / mistakes

- **Conflated "return the verdict" with "return the height."** A function has one return type; when
  a recursion needs to produce two different pieces of information, one of them travels by
  reference. This is the generalisation of the Diameter pattern, and the reason that pattern exists
  at all.
- **Local variable shadowing the function name** — named the wrapper's tracker `isBalanced`, which
  is also the name of the enclosing function. Fixed by renaming it to `balanced`. Logged in
  [BUGWATCH.md](../../BUGWATCH.md).
