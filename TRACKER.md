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
| Trees | [problems/trees/](problems/trees/) | 10 | parked for linked-list detour; next up 102 |
| Linked List | [problems/linked-list/](problems/linked-list/) | 3 + basics | active track (detour, started 2026-09-22) |
| Graphs | [problems/graphs/](problems/graphs/) | 0 | |
| DP | [problems/dp/](problems/dp/) | 0 | |

## Carried-forward questions

Concepts raised but not yet closed out.

- **LC 19 one-pass** — last pasted version still advanced `fast` by `n - 1`; resubmit with `n + 1`
  and confirm. Opened 2026-09-25.
- **Recursive insert at end** (linked-list basics) — scaffolded, never finished. Opened 2026-09-22.
- **Sorting algorithms** — requested 2026-09-23 (how they work, complexity, C++ implementations);
  parked at the opening diagnostic.
- **Unique Paths base cases** — precise conditions at `m-1`, `n-1`. Still open, carried 4 sessions.

### Closed

- ~~**LC 110 Balanced Binary Tree — logged?**~~ — closed 2026-08-22. Now written up at
  [0110](problems/trees/0110-balanced-binary-tree.md); it had been solved but never logged.

- ~~**Pass-by-reference for accumulators**~~ — resolved 2026-08-18 on
  [LC 543](problems/trees/0543-diameter-of-binary-tree.md). Pass-by-value gives each call a private
  copy discarded on return; a reference aliases the caller's memory so updates persist. Confirmed
  against `swap(int&, int&)`. Open since House Robber; spanned 3 sessions and 2 tracks.

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
| 104 | Maximum Depth of Binary Tree | Easy | solved | 2026-08-22 | [0104-maximum-depth-of-binary-tree.md](problems/trees/0104-maximum-depth-of-binary-tree.md) |
| 110 | Balanced Binary Tree | Easy | solved | 2026-08-22 | [0110-balanced-binary-tree.md](problems/trees/0110-balanced-binary-tree.md) |
| 111 | Minimum Depth of Binary Tree | Easy | solved | 2026-08-22 | [0111-minimum-depth-of-binary-tree.md](problems/trees/0111-minimum-depth-of-binary-tree.md) |
| 543 | Diameter of Binary Tree | Easy | solved | 2026-08-18 | [0543-diameter-of-binary-tree.md](problems/trees/0543-diameter-of-binary-tree.md) |
| 572 | Subtree of Another Tree | Easy | solved | 2026-08-22 | [0572-subtree-of-another-tree.md](problems/trees/0572-subtree-of-another-tree.md) |

**Patterns covered:** traversal trio (94/144/145 — root first, middle, last), lockstep two-tree
recursion (100 equality, 101 mirror), dual-purpose helper returning a value while updating a
tracker by reference (543 running max, 110 bool verdict), and pure return-value recursion where the
answer composes from children alone (104, 111).

**Height-helper family:** 543, 110, and 104 all run the same `1 + max(left, right)` recursion. What
changes is what rides along with it — nothing (104), a running max (543), or a bool flag (110).

**Composition:** 572 is the first problem solved by *reusing a previous solution* — LC 100's
`isSameTree` dropped in unchanged as a primitive, with only the outer search written fresh.

**Depth family:** 104 and 111 look symmetric and are not — see
[0111](problems/trees/0111-minimum-depth-of-binary-tree.md) for why swapping `max` for `min`
breaks on one-child nodes.

**Next up:** 102 Level Order — first tree problem that isn't plain recursion (BFS with a queue).

## Linked List

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |
| — | Search key / insert at end (GfG basics) | Easy | attempted | 2026-09-22 | [basics-search-and-insert-at-end.md](problems/linked-list/basics-search-and-insert-at-end.md) |
| 19 | Remove Nth Node From End of List | Medium | solved | 2026-09-25 | [0019-remove-nth-node-from-end-of-list.md](problems/linked-list/0019-remove-nth-node-from-end-of-list.md) |
| 707 | Design Linked List | Medium | solved | 2026-09-29 | [0707-design-linked-list.md](problems/linked-list/0707-design-linked-list.md) |
| 328 | Odd Even Linked List | Medium | solved | 2026-09-29 | [0328-odd-even-linked-list.md](problems/linked-list/0328-odd-even-linked-list.md) |
| 160 | Intersection of Two Linked Lists | Easy | attempted | 2026-09-29 | — |

**Core moves (from 707):** stand on the node *before* the change; save before you break an arrow;
special case handled → update `size` → `return`.

**Pointer walkers:** 19 (fast/slow with a gap) and 328 (two walkers in lockstep, relink then step).
After a walking loop, check where the pointers already stand before writing code to "find" something.

**Dummy node:** 19 one-pass. Removes the head special case by guaranteeing a node before `head`.

**Next up:** 160 Intersection (in progress), then back to Trees at 102 Level Order.

## Graphs

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |

## DP

| # | Problem | Difficulty | Status | Last touched | Write-up |
| --- | --- | --- | --- | --- | --- |
