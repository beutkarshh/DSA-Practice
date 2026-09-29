# 19. Remove Nth Node From End of List

- **Track:** linked-list
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/remove-nth-node-from-end-of-list/
- **Status:** solved (two-pass submitted; one-pass needs confirming)
- **Last touched:** 2026-09-25

## Problem

Remove the n-th node counting from the end and return the head.

## Approach

### Two-pass (brute force)

Count length `L`. The target is at 1-indexed position `L - n + 1`, so its predecessor is at `L - n`,
which is `L - n - 1` steps from `head`. Then `prev->next = prev->next->next`.

Edge case `n == L`: the target is `head` itself, there's no predecessor, so return `head->next`.

### One-pass (fast/slow with a dummy)

Put a `dummy` before `head` and start both pointers there. Move `fast` ahead `n + 1` steps, then move
both until `fast == nullptr`. `slow` lands on the predecessor. The dummy removes the `n == L` special
case, because there is always a node before `head`.

Why it's "better" if both are O(L): it needs no lookahead. Two-pass must know `L` before it can act,
which is impossible on a stream you can't rewind.

## Complexity

- Both: O(L) time, O(1) space

## Solution

```cpp
// Two-pass
ListNode* removeNthFromEnd(ListNode* head, int n) {
    int L = 0;
    ListNode* temp = head;
    while (temp != nullptr) {
        temp = temp->next;
        L++;
    }
    if (n == L) {
        return head->next;
    }
    ListNode* prev = head;
    for (int i = 0; i < L - n - 1; i++) {
        prev = prev->next;
    }
    prev->next = prev->next->next;
    return head;
}

// One-pass
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(0);
    dummy.next = head;
    ListNode* fast = &dummy;
    ListNode* slow = &dummy;
    for (int i = 0; i < n + 1; i++) {
        fast = fast->next;
    }
    while (fast != nullptr) {
        fast = fast->next;
        slow = slow->next;
    }
    slow->next = slow->next->next;
    return dummy.next;
}
```

## Notes / mistakes

- Off-by-one from 0-indexed array thinking: loop ran `L - n` times instead of `L - n - 1`.
- One-pass: brace mismatch closed the function early (`unknown type name 'slow'`).
- One-pass: advance count written as `n - 1` instead of `n + 1`, still in the last pasted version.
  **Resubmit with `n + 1` to confirm.**
