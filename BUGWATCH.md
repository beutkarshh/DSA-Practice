# Bugwatch

Recurring mistakes, not one-off typos. When the same error shows up a second time, promote it
from a problem's "Notes / mistakes" section into a row here — the point is to spot patterns
before an interview does.

| Pattern | Where it bit me | Tell / how to catch it | Times |
| --- | --- | --- | --- |
| **Null-checked the wrong thing** — `p->val == NULL` instead of `p == nullptr`. Asks "is this node's value zero" when the intent is "does this node exist". Distinct from the member-name typo below: the name is real, the *question* is wrong. | LC 100 Same Tree (2026-08-18); linked-list search `head->data == NULL` (2026-09-22) | **Compiles silently** — no compiler help. Tell: you already dereferenced `p` in order to null-check `p`, which is backwards. If `->` appears in a null guard, it's wrong. | 2 |
| **Local variable shadowing the function name** — declared `bool isBalanced` inside the function `isBalanced`. | LC 110 Balanced (2026-08-22) | Compiles or errors confusingly depending on context, and reads fine to a tired eye. Tell: the tracker variable wants the same obvious name as the function computing it. Give the local a distinct name (`balanced`). | 1 |
| **Two jobs, one return** — tried to return the *verdict* from a helper whose return type is the *height*. | LC 110 Balanced (2026-08-22) | A function has one return type. When a recursion must produce two pieces of information, one returns and the other travels by reference. Tell: you're about to write `return false;` in a function declared `int`. | 1 |
| **Case-sensitivity / spelling slip** — `P` for `p`, `subroot` for `subRoot`, `TRUE` for `true`, `addAthead`, `toDelte`. | LC 572 Subtree (2026-08-22), 3 times; linked-list basics (2026-09-22); LC 707 (2026-09-29), twice | Compiler catches it, but it burns submissions and focus. Tell: it clusters when a signature has both a short and a camelCase name. | 6 |
| **Copy-paste without editing** — duplicated a null-check line and left `&&` where the second one needed `\|\|`. | LC 572 Subtree (2026-08-22) | **Compiles and often passes some tests.** Caught here by diffing against the filed 0100 write-up. Tell: two adjacent near-identical lines — read the second one on its own terms, not as a copy of the first. | 1 |
| **Recomputed a subproblem already available** — called `isSameTree(root, subRoot)` once per `if` branch instead of once into the OR chain. | LC 572 Subtree (2026-08-22); avoided on LC 543 | Costs complexity silently, never fails a test. Tell: the same call appears twice with identical arguments. Store it, or restructure so it's evaluated once. | 1 |
| **Guard ordering** — wrote the dereferencing case before the null guards, so the guards never ran. Null-pointer crash. | LC 100 Same Tree (2026-08-18); latent in LC 101 wrapper; LC 111 Min Depth (2026-08-22) | Guards go **first**, before any `->`. Scan top-down: is every dereference below the guard that protects it? **Most persistent pattern in the log.** | 3 |
| **Wrong name for the parameter in scope** — typed `node->left` in a function whose parameter is `root`; earlier, `root` inside `isMirror` whose params are `left`/`right`. Reaching for a name from a *different* function's signature. | LC 101 Symmetric (2026-08-18); LC 104 Max Depth (2026-08-22) | Compiler catches it. Tell: it shows up right after copying a solution shape from a previous problem — the old parameter name comes along with the pattern. Rename deliberately when you reuse a skeleton. | 2 |
| **`&` for `&&`** — bitwise AND where logical AND was meant. | LC 100 Same Tree (2026-08-18), recurred within session | Compiles, and often *works* on bools, which is why it survives. Grep the boolean conditions for single `&`/`\|` before submitting. | 2 |
| **Walker declared inside the loop** — `Node* curr = head;` inside the `for`. Resets to `head` every pass, and `curr` no longer exists after the loop. | LC 707 Design (2026-09-29), 3 times in one sitting | Tell: the only line inside a walking loop should be `curr = curr->next`. Declaration goes once, above. Self-corrected by the end of the session. | 3 |
| **Missing `return` after a special case** — handled the empty/index-0 case, then fell through into the general code. | LC 707 Design (2026-09-29): self-loop, double insert, double delete | Tell: after an `if` that fully handles a case, ask "should anything below still run?" Pattern: handle → update `size` → `return`. | 3 |
| **Return value doesn't match the function's type** — `return -1;` in `void`, `return -1;` in a `ListNode*` function. Extends "two jobs, one return". | LC 707 (2026-09-29); LC 328 (2026-09-29); cf. LC 110 | Read the signature before writing any `return`. `void` → `return;`; pointer → a pointer (`nullptr`/`head`). | 2 |
| **`=` inside a condition** — `if (head = nullptr)`. | LC 707 Design (2026-09-29) | **Compiles.** Assigns, wipes the list, and the branch never runs. Grep every `if`/`while` for a single `=`. | 1 |
| **Off-by-one on a walk count** — steps to reach a position vs. the position itself (`L - n` vs `L - n - 1`, `size` vs `size - 1`, `n - 1` vs `n + 1`). | LC 19 (2026-09-25), twice; LC 707 addAtTail (2026-09-29) | Trace the smallest list by hand: where does the walker *stop*? Steps from `head` to 0-indexed `i` is exactly `i`. | 3 |
| **Rewired before saving** — changed an arrow, then tried to read where it used to point. | LC 707 addAtIndex (2026-09-29), twice | Tell: the second line reads `curr->next` after the first line changed it. New node grabs the old arrow first. | 2 |
| **Invented member/field name** — reaching for a plausible-sounding name instead of the real one (`node->value` for `node->val`). Not an operator or index confusion; the name simply doesn't exist. | LC 144 Preorder (2026-08-18); LC 707 `curr->value` (2026-09-29) | Compiler catches it, but the fix is to go **reread the struct/class definition** rather than guess a second time. Under time pressure the guess feels certain — that certainty is the tell. | 2 |

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
- [ ] Height base case is **`height(NULL) = 0`**, not "leaf = 0". A leaf's 1 is *derived*
      (`1 + max(0,0)`). Anchoring on the leaf instead of the null child is the usual off-by-one here.
- [ ] Path lengths: edges or nodes? Diameter counts **edges**, so `leftHeight + rightHeight` with no
      `+1`. Reread the problem statement before adding a constant.
- [ ] Called a recursive helper twice on the same child instead of storing the result. Compiles, passes,
      silently costs an exponential blowup. If `height(node->left, ...)` appears twice, store it.
- [ ] Accumulator/tracker parameter declared without `&`. Closed as a concept on LC 543, but the
      *typo* stays easy — a missing `&` compiles fine and quietly returns the initial value.
- [ ] A NULL child returns 0 — harmless under `max`, **fatal under `min`** (0 always wins, so a
      one-child node falsely reports as a leaf). Never assume `max`→`min` gives the mirror problem.
- [ ] "Leaf" means *no children*. A one-child node is not a leaf; don't let a path terminate there.
- [ ] Whole-tree properties (balance, BST-ness) must be checked at **every** node, not just the root.
      Ride the check along inside an existing O(n) traversal rather than recomputing per node (O(n^2)).
- [ ] A one-way flag (true → false, never back) needs no short-circuit — but confirm nothing resets it.
- [ ] Reusing a solved problem as a primitive? Its base case may not transfer — `isSameTree` says
      "both null → true", `isSubtree` says "null root → false". Different question, different empty case.
- [ ] `return A || B || C;` — not `if (A||B||C) return true; else return false;`. The branch form
      invites evaluating `A` twice.

### Linked List
- [ ] Walking loop: `curr` declared **once above** the loop; only `curr = curr->next` inside.
- [ ] Insert/delete at `i` → stand on node `i - 1`. Index 0 (or empty list) is the special case — or use a dummy.
- [ ] Save before you break: `newNode->next = curr->next;` **then** `curr->next = newNode;`.
- [ ] Deleting: save `toDelete` before rerouting, then `delete toDelete;`.
- [ ] Add allows `index == size`; delete needs `index < size`.
- [ ] `while (a != nullptr && a->next != nullptr)` — null check on the left, or it crashes.
- [ ] Never move `head` to walk; copy it into `curr`/`temp`.
- [ ] After a loop, ask where the walkers already stand before writing code to find something.

### Graphs
- [ ] Marking visited at dequeue instead of enqueue — duplicates in the queue.

### DP
- [ ] Iteration order doesn't respect dependency order of the recurrence.
