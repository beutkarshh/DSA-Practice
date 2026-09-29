# 707. Design Linked List

- **Track:** linked-list
- **Difficulty:** Medium
- **Link:** https://leetcode.com/problems/design-linked-list/
- **Status:** solved
- **Last touched:** 2026-09-29

## Problem

Build a singly linked list from scratch: `get`, `addAtHead`, `addAtTail`, `addAtIndex`,
`deleteAtIndex`, all 0-indexed, with invalid indexes ignored (or `-1` for `get`).

## Approach

The list keeps two things: `head` (the first node, `nullptr` when empty) and `size`. Keeping `size`
makes every validity check instant (`index >= size`) instead of a walk.

### Rule 1: stand on the node *before*

Arrows only point forward. To insert or delete at index `i`, the arrow that changes belongs to the
node at `i - 1`, so walk `i - 1` steps and stop there. Index 0 has no "before" node, so it's the
special case in both `addAtIndex` and `deleteAtIndex`.

### Rule 2: save before you break

Connect the new node first, then move the old arrow:

```cpp
newNode->next = curr->next;   // grab the rest of the list
curr->next = newNode;         // then rewire
```

Reversed, `curr->next = newNode` runs first and the second line reads the *new* arrow, so the node
points to itself. For deletes, save `toDelete` before rerouting so it can still be freed.

### Rule 3: special case handled → update `size` → `return`

Every bug-free special-case block ends this way. Without the `return`, the general code below runs
too (double insert, self-loop, two deletes).

### Add allows `index == size`, delete doesn't

There's a slot after the last node to add into; there's no node at `size` to delete. So add checks
`index > size`, delete checks `index >= size`. And `addAtIndex(size, val)` *is* add-at-tail, which
lets `addAtTail` be one line.

## Complexity

- `addAtHead`: O(1)
- `get`, `addAtTail`, `addAtIndex`, `deleteAtIndex`: O(n) (the walk)
- Space: O(n) for n nodes

## Solution

```cpp
struct Node {
    int val;
    Node* next;
    Node(int x) : val(x), next(nullptr) {}
};

class MyLinkedList {
private:
    Node* head;
    int size;

public:
    MyLinkedList() {
        head = nullptr;
        size = 0;
    }

    int get(int index) {
        if (index >= size) return -1;
        Node* curr = head;
        for (int i = 0; i < index; i++) {
            curr = curr->next;
        }
        return curr->val;
    }

    void addAtHead(int val) {
        Node* newNode = new Node(val);
        newNode->next = head;
        head = newNode;
        size++;
    }

    void addAtTail(int val) {
        addAtIndex(size, val);
    }

    void addAtIndex(int index, int val) {
        if (index > size) return;
        if (index == 0) {
            addAtHead(val);
            return;
        }
        Node* newNode = new Node(val);
        Node* curr = head;
        for (int i = 0; i < index - 1; i++) {
            curr = curr->next;
        }
        newNode->next = curr->next;
        curr->next = newNode;
        size++;
    }

    void deleteAtIndex(int index) {
        if (index >= size) return;
        if (index == 0) {
            Node* toDelete = head;
            head = head->next;
            delete toDelete;
            size--;
            return;
        }
        Node* curr = head;
        for (int i = 0; i < index - 1; i++) {
            curr = curr->next;
        }
        Node* toDelete = curr->next;
        curr->next = toDelete->next;
        delete toDelete;
        size--;
    }
};
```

## Notes / mistakes

- **`Node* curr = head` inside the walking loop**, three times (`get`, `addAtTail`, `addAtIndex`).
  Resets the walker every pass *and* scopes `curr` to the loop so the line after it won't compile.
  Fixed on its own by `deleteAtIndex`.
- **Missing `return` after a special case**, three times (`addAtTail` empty case → self-loop,
  `addAtIndex` index 0 → double insert, `deleteAtIndex` index 0 → deleted two nodes).
- **Double `size++`** after calling `addAtHead`, which already counts.
- **`if (head = nullptr)`**: assignment, not comparison. Wipes the list and the `if` is always false.
- **Wrong linking order** in `addAtIndex`, twice: first lost the tail, then made a self-loop.
- `return -1;` in a `void` function; `curr->value` for `curr->val`; `addAthead`; `toDelte`/`toDelete`.
- `head = Node* curr;` written backwards. Declarations are `type name = value;`, and `=` copies right
  into left.
- `deleteAtIndex` bound slipped from correct (`size - 1`) to `index > size` on a retype.
