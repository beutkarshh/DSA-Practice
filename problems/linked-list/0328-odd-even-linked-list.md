# 328. Odd Even Linked List

- **Track:** linked-list
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/odd-even-linked-list/
- **Status:** solved
- **Last touched:** 2026-09-29

## Problem

Regroup the list so all nodes at odd **positions** (1st, 3rd, ...) come first, then all even
positions, each group keeping its original order. O(n) time, O(1) extra space. Positions, not values.

## Approach

O(1) space rules out copying, so only arrows get rewired. Split into two chains in one pass, then
glue the odd chain's tail to the even chain's head.

```
[1] → [2] → [3] → [4] → [5]
odd:  [1] → [3] → [5]
even: [2] → [4]
glue: [1] → [3] → [5] → [2] → [4]
```

### Save the even head first

After the first rewire (`[1] → [3]`), nothing points to `[2]` anymore. Save `evenHead = head->next`
before touching anything; the glue step needs it.

### Two walkers, relink then step

```cpp
odd->next = even->next;  odd = odd->next;
even->next = odd->next;  even = even->next;
```

Line 3 depends on `odd` having already moved.

### Loop condition

`while (even != nullptr && even->next != nullptr)`. Odd length ends with `even == nullptr`; even
length ends with `even->next == nullptr` (another round would move `odd` onto null). Left-to-right
short-circuit means `even->next` is never read on a null `even`.

### The walker already found the tail

No search needed for the last odd node: when the loop stops, `odd` is standing on it.

## Complexity

- Time: O(n), one pass
- Space: O(1), three pointers

## Solution

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if (head == nullptr) {
            return head;
        }
        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* evenHead = even;

        while (even != nullptr && even->next != nullptr) {
            odd->next = even->next;
            odd = odd->next;
            even->next = odd->next;
            even = even->next;
        }

        odd->next = evenHead;
        return head;
    }
};
```

## Brute force (for comparison)

Collect odd-position and even-position values into two vectors, then write them back over the
nodes. O(n) time but O(n) space, which breaks the problem's constraint, and it repaints values
instead of moving nodes. Worth *saying* in an interview before coding the pointer version.

## Notes / mistakes

- `return -1;` from a function returning `ListNode*`. Empty list → `return head;`.
- Didn't see that `odd` already sits on the last odd node; asked how to "find" it.
