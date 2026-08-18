# Bugwatch

Recurring mistakes, not one-off typos. When the same error shows up a second time, promote it
from a problem's "Notes / mistakes" section into a row here — the point is to spot patterns
before an interview does.

| Pattern | Where it bit me | Tell / how to catch it | Times |
| --- | --- | --- | --- |
| **Null-checked the wrong thing** — `p->val == NULL` instead of `p == nullptr`. Asks "is this node's value zero" when the intent is "does this node exist". Distinct from the member-name typo below: the name is real, the *question* is wrong. | LC 100 Same Tree (2026-08-18) | **Compiles silently** — no compiler help. Tell: you already dereferenced `p` in order to null-check `p`, which is backwards. If `->` appears in a null guard, it's wrong. | 1 |
| **Guard ordering** — wrote the dereferencing case before the null guards, so the guards never ran. Null-pointer crash. | LC 100 Same Tree (2026-08-18); latent in LC 101 wrapper | Guards go **first**, before any `->`. Scan top-down: is every dereference below the guard that protects it? | 2 |
| **`&` for `&&`** — bitwise AND where logical AND was meant. | LC 100 Same Tree (2026-08-18), recurred within session | Compiles, and often *works* on bools, which is why it survives. Grep the boolean conditions for single `&`/`\|` before submitting. | 2 |
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
- [ ] Null guards placed **above** every dereference, wrapper functions included.
- [ ] Prefer `nullptr` over `NULL` — `NULL` is an integer constant and invites the `->val == NULL` bug.
- [ ] Two-tree recursion: which children pair up? Same-position (`l->left`/`r->left`) for equality,
      crossed (`l->left`/`r->right`) for mirror. Picking the wrong one silently solves a different problem.
- [ ] Calling the one-arg wrapper where the two-arg helper was meant — check arity at every recursive call.

### Graphs
- [ ] Marking visited at dequeue instead of enqueue — duplicates in the queue.

### DP
- [ ] Iteration order doesn't respect dependency order of the recurrence.
