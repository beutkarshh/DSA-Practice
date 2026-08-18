# Bugwatch

Recurring mistakes, not one-off typos. When the same error shows up a second time, promote it
from a problem's "Notes / mistakes" section into a row here — the point is to spot patterns
before an interview does.

| Pattern | Where it bit me | Tell / how to catch it | Times |
| --- | --- | --- | --- |
| **Invented member/field name** — reaching for a plausible-sounding name instead of the real one (`node->value` for `node->val`). Not an operator or index confusion; the name simply doesn't exist. | LC 144 Preorder (2026-08-18) | Compiler catches it, but the fix is to go **reread the struct/class definition** rather than guess a second time. Under time pressure the guess feels certain — that certainty is the tell. | 1 |

## Per-track watchlist

### Recursion
- [ ] Base case missing or unreachable — check the recursion actually terminates on the smallest input.
- [ ] Memo keyed on the wrong state (missing a dimension that varies).
- [ ] Mutating shared state across branches without undoing it on the way out.

### Arrays & Two Pointers
- [ ] Off-by-one on the loop bound / pointer crossing condition.

### Binary Search
- [ ] `lo <= hi` vs `lo < hi` mismatched with how `mid` is updated — infinite loop.
- [ ] Overflow-prone `(lo + hi) / 2` in fixed-width languages.

### Sliding Window
- [ ] Shrinking the window before recording the answer (or after, when it should be before).

### Trees
- [ ] Not handling the null/None node before dereferencing.
- [ ] `node->val` — check the member name against the given struct, don't type it from memory.

### Graphs
- [ ] Marking visited at dequeue instead of enqueue — duplicates in the queue.

### DP
- [ ] Iteration order doesn't respect dependency order of the recurrence.
