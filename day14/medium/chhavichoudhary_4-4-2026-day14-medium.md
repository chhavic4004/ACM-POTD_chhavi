# LC 24. Swap Nodes in Pairs

**Difficulty:** Medium
**Topic:** Linked List, Recursion
**Author:** Chhavi

---

## Problem Statement

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem **without modifying the values** in the list's nodes (i.e., only nodes themselves may be changed.)

**Constraints:**
- Number of nodes in range `[0, 100]`
- `0 <= Node.val <= 100`

---

## Approach — Pure Recursion

### Intuition

Every pair-swap problem has a self-similar structure: swap the first two nodes, then do the exact same thing to the rest of the list. That is recursion's natural domain.

For any call `swapPairs(head)`, `second` becomes the new local head, `first` drops behind it, and `first->next` chains to whatever the recursive call returns for the rest. The base case handles empty lists and single nodes — both are already "swapped."

### Steps

1. If `head` is null or `head->next` is null → return `head` (nothing to swap).
2. Mark `first = head`, `second = head->next`.
3. Recursively solve the rest: `first->next = swapPairs(second->next)`.
4. Link second back to first: `second->next = first`.
5. Return `second` as the new head of this pair.

### Key Order of Operations

```
first->next = swapPairs(second->next)   ← recurse BEFORE rewiring second
second->next = first                    ← now safely point second at first
return second                           ← second is the new head
```

If these two lines are flipped, `second->next = first` overwrites `second->next` before it is passed into the recursive call — and the rest of the list is lost.

---

## Code

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode* first = head;
        ListNode* second = head->next;
        first->next = swapPairs(second->next);
        second->next = first;
        return second;
    }
};
```

---

## Dry Run

### Example 1: `head = [1, 2, 3, 4]`

```
swapPairs(1→2→3→4)
  first = 1, second = 2
  first->next = swapPairs(3→4)
                    first = 3, second = 4
                    first->next = swapPairs(null) = null
                    second->next = first  →  4→3→null
                    return 4
  first->next = 4→3  →  1→4→3
  second->next = first  →  2→1→4→3
  return 2
```

**Output:** `[2, 1, 4, 3]` ✓

---

### Example 2: `head = [1, 2, 3]`

```
swapPairs(1→2→3)
  first = 1, second = 2
  first->next = swapPairs(3)
                    !head->next → return 3   (base case)
  first->next = 3  →  1→3
  second->next = first  →  2→1→3
  return 2
```

**Output:** `[2, 1, 3]` ✓

---

### Example 3: `head = []` or `head = [1]`

Both hit the base case `!head || !head->next` and return `head` immediately.

**Output:** `[]` / `[1]` ✓

---

## Complexity Analysis

| | Complexity | Reason |
|---|---|---|
| **Time** | O(n) | Each node visited exactly once across all recursive calls |
| **Space** | O(n/2) | One stack frame per pair; n/2 pairs total |

---

## Edge Cases

| Case | Input | Expected Output | Handled By |
|------|-------|-----------------|------------|
| Empty list | `[]` | `[]` | `!head` base case |
| Single node | `[1]` | `[1]` | `!head->next` base case |
| Two nodes | `[1, 2]` | `[2, 1]` | One recursive call + base case (null) |
| Odd length | `[1, 2, 3]` | `[2, 1, 3]` | Unpaired last node returned as-is by base case |
| Even length | `[1, 2, 3, 4]` | `[2, 1, 4, 3]` | All pairs swapped cleanly |
| All same values | `[5, 5, 5, 5]` | `[5, 5, 5, 5]` | Nodes swapped structurally, values unchanged |

---



---

![screenshot](chhavichoudhary_4-4-2026-day14-medium.png)