# 111. Minimum Depth of Binary Tree

- **Track:** trees
- **Difficulty:** Easy
- **Link:** https://leetcode.com/problems/minimum-depth-of-binary-tree/
- **Status:** solved
- **Last touched:** 2026-08-22

## Problem

Return the number of nodes along the shortest path from the root down to the **nearest leaf** —
where a leaf is a node with *no* children.

## Approach

### The trap

The obvious move is to take [Max Depth](0104-maximum-depth-of-binary-tree.md) and swap `max` for
`min`. **That is wrong**, and it's the entire point of the problem.

A NULL child returns 0. For `max` that's harmless — 0 never wins. For `min` it's fatal: 0 always
wins. So a node with one real child and one missing child would report depth 1, claiming it's a
leaf when it isn't. The recursion cannot tell "no path this way" apart from "a path ending here",
because both come back as 0.

The definition is what breaks it: a leaf has *no children*. A one-child node is not a leaf, so no
shortest path is allowed to stop there.

### The fix

Handle the missing child explicitly instead of letting `min` see its 0. Null guard, then four
cases per node:

1. `root == NULL` -> `0` (empty tree)
2. **Leaf** — both children NULL -> `1`
3. **Only left exists** (`right == NULL`) -> `1 + minDepth(left)`
4. **Only right exists** (`left == NULL`) -> `1 + minDepth(right)`
5. **Both exist** -> `1 + min(minDepth(left), minDepth(right))`

`min` appears in exactly one branch — case 5, the only place both sides are genuine paths. Cases 3
and 4 don't compare at all; they descend the one way that exists.

Case 2 is technically redundant (case 5 can't fire, and 3/4 would return `1 + 0`), but writing it
out states the leaf definition explicitly, which is the concept the problem is actually testing.

## Complexity

- Time: O(n)
- Space: O(h) recursion stack

## Solution

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (root == NULL) return 0;
        else if (root->left == NULL && root->right == NULL) return 1;
        else if (root->right == NULL) return 1 + minDepth(root->left);
        else if (root->left == NULL) return 1 + minDepth(root->right);
        else return 1 + min(minDepth(root->left), minDepth(root->right));
    }
};
```

Worth noting: this is the first problem in the track where **max and min are not symmetric**. It's
tempting to assume an operator swap gives you the mirror-image problem; here the asymmetry comes
from what the base case returns, not from the operator.

## Notes / mistakes

- **Missing `root == NULL` guard.** The first version dereferenced `root->left` before checking
  `root`, so an empty tree would crash. Third appearance of the guard-ordering pattern — LC 100,
  latent in LC 101's wrapper, now here. See [BUGWATCH.md](../../BUGWATCH.md).
- **Parameter-naming slip** — wrote `node->left` where the parameter is `root`. Second appearance;
  also hit on [LC 104](0104-maximum-depth-of-binary-tree.md) this session.
