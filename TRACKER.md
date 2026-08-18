# DSA Practice Tracker

Progress log across tracks. One row per problem; link the write-up in `problems/<track>/`.

**Status legend:** `not started` · `attempted` · `solved` · `needs review`

## Tracks

| Track | Folder | Solved | Notes |
| --- | --- | --- | --- |
| Arrays & Two Pointers | [problems/arrays-two-pointers/](problems/arrays-two-pointers/) | 0 | |
| Binary Search | [problems/binary-search/](problems/binary-search/) | 0 | |
| Sliding Window | [problems/sliding-window/](problems/sliding-window/) | 0 | |
| Recursion | [problems/recursion/](problems/recursion/) | 2 | write-ups still empty; 2 open questions |
| Trees | [problems/trees/](problems/trees/) | 5 | active track |
| Graphs | [problems/graphs/](problems/graphs/) | 0 | |
| DP | [problems/dp/](problems/dp/) | 0 | |

## Carried-forward questions

Concepts raised but not yet closed out. Clear these before going deeper into Trees.

- **Pass-by-reference for accumulators** — why `vector<int>&` and not `vector<int>`. Opened on
  House Robber's `dp`, resurfaced on LC 94/144, still open after session 2 of Trees. Reasoning is
  written up in [0094](problems/trees/0094-binary-tree-inorder-traversal.md); wants explicit confirmation.
- **Unique Paths base cases** — precise conditions at `m-1`, `n-1`. Still open, carried 2 sessions.

## Recursion

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |
| 62 | Unique Paths | Medium | needs review | — | [0062-unique-paths.md](problems/recursion/0062-unique-paths.md) |
| 198 | House Robber | Medium | needs review | — | [0198-house-robber.md](problems/recursion/0198-house-robber.md) |

## Arrays & Two Pointers

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |

## Binary Search

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |

## Sliding Window

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |

## Trees

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |
| 94 | Binary Tree Inorder Traversal | Easy | solved | 2026-08-18 | [0094-binary-tree-inorder-traversal.md](problems/trees/0094-binary-tree-inorder-traversal.md) |
| 100 | Same Tree | Easy | solved | 2026-08-18 | [0100-same-tree.md](problems/trees/0100-same-tree.md) |
| 101 | Symmetric Tree | Easy | solved | 2026-08-18 | [0101-symmetric-tree.md](problems/trees/0101-symmetric-tree.md) |
| 144 | Binary Tree Preorder Traversal | Easy | solved | 2026-08-18 | [0144-binary-tree-preorder-traversal.md](problems/trees/0144-binary-tree-preorder-traversal.md) |
| 145 | Binary Tree Postorder Traversal | Easy | solved | 2026-08-18 | [0145-binary-tree-postorder-traversal.md](problems/trees/0145-binary-tree-postorder-traversal.md) |

**Patterns covered:** traversal trio (94/144/145 — root first, middle, last) and lockstep two-tree
recursion (100 equality, 101 mirror).

**Next up:** 104 Maximum Depth — first tree problem where the recursion returns a *computed value*
(an int built from both children's results) rather than a bool or an accumulator. Bridges toward
height/depth problems. Then 102 Level Order.

## Graphs

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |

## DP

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |
