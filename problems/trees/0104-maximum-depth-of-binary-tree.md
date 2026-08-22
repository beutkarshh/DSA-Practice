# 104. Maximum Depth of Binary Tree

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/maximum-depth-of-binary-tree/
- **Status:** solved
- **Last touched:** 2026-08-22

## Problem

Return the number of nodes along the longest path from the root down to the farthest leaf.

## Approach

The simplest shape in the Trees track so far: **no tracker, no reference parameter, no helper.**
The answer flows straight up through return values.

That's the contrast worth holding onto. [Diameter](0543-diameter-of-binary-tree.md) needed
`int &diameter` because the answer was a *running best across the whole tree* — a path bending at
some node that the return value couldn't carry. Here the answer at each node is a pure function of
its children's answers, so returning it is enough.

```
maxDepth(NULL) = 0
maxDepth(node) = 1 + max(maxDepth(left), maxDepth(right))
```

This is the `height` helper from Diameter, renamed. Same recursion, same base case.

### Nodes, not edges

Depth here counts **nodes**. Diameter counted **edges**. The recursion is identical — what differs
is where you start counting, and that falls out of the base case: `NULL -> 0` means every real node
adds itself via the `+ 1`. Anchor on the null child and node-counting is automatic.

## Complexity

- Time: O(n)
- Space: O(h) recursion stack

## Solution

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL) return 0;
        int leftHeight = maxDepth(root->left);
        int rightHeight = maxDepth(root->right);
        return 1 + max(leftHeight, rightHeight);
    }
};
```

## Notes / mistakes

- Parameter-naming slip: reached for `node->left` when the parameter is called `root`. Same class of
  error as writing `root` inside `isMirror` on [LC 101](0101-symmetric-tree.md), where the params
  were `left`/`right`. Logged in [BUGWATCH.md](../../BUGWATCH.md).
