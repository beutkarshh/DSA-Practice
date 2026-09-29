# Linked List Basics: Search and Insert at End

- **Track:** linked-list
- **Source:** GeeksforGeeks-style practice (not LeetCode)
- **Status:** search solved (iterative + recursive); insert at end solved iterative, recursive open
- **Last touched:** 2026-09-22

## Search for a key

Iterative (written independently, no bugs):

```cpp
bool searchKey(Node* head, int key) {
    Node* curr = head;
    while (curr != NULL) {
        if (curr->data == key) {
            return true;
        }
        curr = curr->next;
    }
    return false;
}
```

Recursive: base cases are "ran off the list" (`head == NULL` → false) and "found it here" (→ true);
otherwise `return searchKey(head->next, key);`. The `true` rides back up the call chain untouched.

```cpp
bool searchKey(Node* head, int key) {
    if (head == NULL) return false;
    if (head->data == key) return true;
    return searchKey(head->next, key);
}
```

| | Iterative | Recursive |
| --- | --- | --- |
| Move forward | `curr = curr->next` | call again with `head->next` |
| Time | O(n) | O(n) |
| Space | O(1) | O(n) call stack |

## Insert at end

Iterative: empty list → the new node becomes `head`. Otherwise walk to the last node
(`curr->next == NULL`) and attach. Solved.

Recursive: scaffolded, not finished. **Open.**

## Notes / mistakes

- `head->data == NULL` instead of `head == NULL` (null-checked the wrong thing).
- `TRUE` instead of `true`.
- `curr-next` instead of `curr->next`.
