# LC 61. Rotate List

**Difficulty:** Medium
**Topic:** Linked List, Two Pointers
**Author:** Chhavi

---

## Problem Statement

Given the `head` of a linked list, rotate the list to the right by `k` places.

**Constraints:**
- Number of nodes in range `[0, 500]`
- `-100 <= Node.val <= 100`
- `0 <= k <= 2 * 10^9`

---

## Banned Solution

> Find length → make list circular → walk to new tail position → break the circle.

---

## Approach — Two Pointer (Fast & Slow with gap of k)

### Intuition

Rotating right by `k` means the last `k` nodes move to the front. So we need to find:
- The **new tail** — the node just before where the rotation cuts
- The **new head** — the node right after the new tail
- The **old tail** — which must point back to the old head

If we place `fast` exactly `k` steps ahead of `slow`, both starting at `head`, then when `fast` reaches the last node (`fast->next == null`), `slow` is sitting exactly at the **new tail**. One pass after the gap is established gives us all three pointers we need.

### Why `k %= len` first?

`k` can be up to `2 * 10^9` but rotating by `len` is a no-op. So effective rotation = `k % len`. If `k % len == 0`, return early.

### Steps

1. Compute `len` with one full traversal.
2. Reduce `k %= len`. If 0, return early.
3. Place `fast` at index `k` (i.e., `k` steps ahead of `slow` at `head`).
4. Advance both until `fast->next == nullptr`.
5. At this point:
   - `slow` = new tail
   - `slow->next` = new head
   - `fast` = old tail
6. Rewire: `fast->next = head`, `slow->next = nullptr`, return `newHead`.

---

## Code

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || !head->next) return head;

        int len = 0;
        ListNode* temp = head;
        while (temp) { len++; temp = temp->next; }

        k %= len;
        if (k == 0) return head;

        ListNode* fast = head;
        ListNode* slow = head;

        for (int i = 0; i < k; i++) fast = fast->next;

        while (fast->next) {
            fast = fast->next;
            slow = slow->next;
        }

        ListNode* newHead = slow->next;
        slow->next = nullptr;
        fast->next = head;
        return newHead;
    }
};
```

---

## Dry Run

### Example 1: `head = [1, 2, 3, 4, 5], k = 2`

**len = 5, k = 2 % 5 = 2**

After gap setup (`fast` moves 2 steps):
```
slow = node(1)
fast = node(3)
```

Advance both until `fast->next == null`:

| Step | slow | fast |
|------|------|------|
| start | 1 | 3 |
| 1 | 2 | 4 |
| 2 | 3 | 5 ← fast->next is null, stop |

State at stop:
- `slow` = node(3) → **new tail**
- `slow->next` = node(4) → **new head**
- `fast` = node(5) → **old tail**

Rewire:
- `fast->next = head` → node(5) → node(1)
- `slow->next = nullptr` → node(3) → null

**Output:** `[4, 5, 1, 2, 3]` ✓

---

### Example 2: `head = [0, 1, 2], k = 4`

**len = 3, k = 4 % 3 = 1**

After gap setup (`fast` moves 1 step):
```
slow = node(0)
fast = node(1)
```

Advance both until `fast->next == null`:

| Step | slow | fast |
|------|------|------|
| start | 0 | 1 |
| 1 | 1 | 2 ← fast->next is null, stop |

State at stop:
- `slow` = node(1) → **new tail**
- `slow->next` = node(2) → **new head**
- `fast` = node(2) → **old tail**

Rewire:
- `fast->next = head` → node(2) → node(0)
- `slow->next = nullptr` → node(1) → null

**Output:** `[2, 0, 1]` ✓

---

## Complexity Analysis

| | Complexity | Reason |
|---|---|---|
| **Time** | O(n) | One pass to find length + one pass for two-pointer walk |
| **Space** | O(1) | Only pointer variables, no extra data structures |

---

## Edge Cases

| Case | Input | Expected Output | Handled By |
|------|-------|-----------------|------------|
| Empty list | `[], k = 3` | `[]` | `!head` check |
| Single node | `[1], k = 5` | `[1]` | `!head->next` check |
| k = 0 | `[1,2,3], k = 0` | `[1,2,3]` | `k %= len` → 0 → early return |
| k = multiple of len | `[1,2,3], k = 6` | `[1,2,3]` | `k %= len` → 0 → early return |
| k > len | `[0,1,2], k = 4` | `[2,0,1]` | `k %= len` reduces to effective rotation |
| k = len - 1 | `[1,2,3], k = 2` | `[2,3,1]` | fast starts 1 behind tail; slow ends at head |

---

## Difference from Banned Solution

| Aspect | Banned (Circular rewire) | This (Two Pointer) |
|---|---|---|
| Core mechanic | Make list circular, walk to break point | Maintain k-gap between fast and slow |
| Pointer ops | Tail → head (circular), then break | fast→head, slow→null (no intermediate circle) |
| New tail found by | Walking `length - k - 1` steps from head | Advancing both until fast->next is null |
| Traversals | 1 (length) + 1 (walk to break) | 1 (length) + 1 (two-pointer walk) |
| Space | O(1) | O(1) |

![screenshot](chhavichoudhary_2-4-2026-day12-intermediate.png)