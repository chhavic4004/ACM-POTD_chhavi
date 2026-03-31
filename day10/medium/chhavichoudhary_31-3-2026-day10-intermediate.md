# 2. Add Two Numbers

**Difficulty:** Medium  
**Topic:** Linked List, Math  
**Author:** Chhavi

---

## Problem

You are given two non-empty linked lists representing two non-negative integers. Digits are stored in **reverse order**, each node contains a single digit. Add the two numbers and return the sum as a linked list.

---

## My Approach

**Vector-based digit addition (four-phase simulation)**

Converting to integers fails for 100-node lists (100-digit numbers overflow even `__int128`). Instead, simulate paper addition using vectors:

1. **Dump** both lists into vectors (`v1`, `v2`) — digits already in least-significant-first order.
2. **Pad** the shorter vector with trailing zeros so both are the same length.
3. **Sum pass** — add corresponding digits index by index into a `result` vector (values may exceed 9 here).
4. **Carry pass** — propagate carries left-to-right through `result`; append an extra node if the last digit still overflows.
5. **Rebuild** — walk `result` and create linked list nodes.

---

## Code

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        vector<int> v1, v2;

        while (l1) { v1.push_back(l1->val); l1 = l1->next; }
        while (l2) { v2.push_back(l2->val); l2 = l2->next; }

        int maxLen = max(v1.size(), v2.size());
        v1.resize(maxLen, 0);
        v2.resize(maxLen, 0);

        vector<int> result(maxLen, 0);

        for (int i = 0; i < maxLen; i++)
            result[i] = v1[i] + v2[i];

        for (int i = 0; i < maxLen - 1; i++) {
            result[i + 1] += result[i] / 10;
            result[i] %= 10;
        }

        if (result.back() >= 10) {
            result.push_back(result.back() / 10);
            result[result.size() - 2] %= 10;
        }

        ListNode* dummy = new ListNode(0);
        ListNode* curr = dummy;
        for (int digit : result) {
            curr->next = new ListNode(digit);
            curr = curr->next;
        }

        return dummy->next;
    }
};
```

---

## Complexity

| | Value |
|---|---|
| Time Complexity | O(m + n) |
| Space Complexity | O(max(m, n)) |

---

![screenshot](chhavichoudhary_31-3-2026-day10-intermediate.png)