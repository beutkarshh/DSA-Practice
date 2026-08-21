# 543. Diameter of Binary Tree

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/diameter-of-binary-tree/
- **Status:** solved
- **Last touched:** 2026-08-18

## Problem

Return the length of the longest path between any two nodes in the tree, measured in **edges**.
The path need not pass through the root.

## Approach

A **new recursive shape**: a dual-purpose helper that returns one thing while updating another as
a side effect.

- The **return value** is the height of the subtree.
- The **side effect** is updating a `diameter` tracker held by reference.

That split is the trick. The answer we want (diameter) isn't the thing that composes cleanly up
the recursion — height is. So height is what gets returned, and the diameter is accumulated on
the way back up.

### Height

```
height(NULL) = 0
height(node) = 1 + max(height(left), height(right))
```

The base case is **`height(NULL) = 0`**, not "leaf height = 0". A leaf's height of 1 is *derived*:
`1 + max(0, 0)`. Getting this backwards is what makes the off-by-one errors in this family of
problems.

### Diameter through a node

For any node, the longest path *bending at that node* is `leftHeight + rightHeight` — in edges.
No `+ 1`, no `+ 2`: each subtree's height already counts the edge up to this node. Every node gets
a turn as the bend point, so taking the running max over all of them covers every possible path,
including ones that never touch the root.

### Compute each child's height exactly once

`leftHeight` and `rightHeight` go into variables and get used **twice** — once for the diameter
check, once for the return value. Calling `height(node->left, ...)` a second time instead of
reusing the stored value would recompute an entire subtree and blow the complexity up. Same
principle as binary exponentiation: never recompute a subproblem you already have.

### Why `int &diameter`

Pass-by-value would give every recursive call its own private copy, discarded on return — the
updates would evaporate and `diameterOfBinaryTree` would always return 0. The reference makes the
parameter an alias for the caller's actual variable, so all frames write to the same memory and
the result survives after the calls unwind. **See the resolved note below.**

## Complexity

- Time: O(n) — each node visited once
- Space: O(h) recursion stack

## Solution

```cpp
class Solution {
public:
    int height(TreeNode* node, int &diameter) {
        if (node == NULL) return 0;

        int leftHeight = height(node->left, diameter);
        int rightHeight = height(node->right, diameter);

        diameter = max(diameter, leftHeight + rightHeight);

        return 1 + max(leftHeight, rightHeight);
    }

    int diameterOfBinaryTree(TreeNode* root) {
        int diameter = 0;
        height(root, diameter);
        return diameter;
    }
};
```

Note the wrapper discards `height`'s return value entirely — it only wants the side effect. That
reads oddly at first but is correct: the height was scaffolding, the diameter is the answer.

## RESOLVED: why accumulators are passed by reference

Open since House Robber's `dp`, resurfaced on LC 94/144, **closed here.**

Pass-by-value hands each function call a private photocopy that is discarded when the call
returns. Pass-by-reference makes the parameter an *alias* for the original variable's memory, so
updates persist across all recursive calls and remain visible to the caller afterward. Confirmed
against a `swap(int&, int&)` example, where the whole point is that the caller's variables change.

Same reason `vector<int>& result` was needed in the traversal problems
([0094](0094-binary-tree-inorder-traversal.md)) — one shared vector, not one per stack frame.

## Notes / mistakes

- None logged this round.
- Contains [LC 104 Maximum Depth](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
  outright — the `height` helper *is* that problem's solution.
