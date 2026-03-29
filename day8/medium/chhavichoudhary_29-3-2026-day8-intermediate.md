# 19. Remove Nth Node From End of List

**Author:** Chhavi  
**Platform:** LeetCode  
**Difficulty:** Medium  
**Language:** C++

---

## Problem

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

---

## My Approach

I used the **two-pointer (fast & slow) technique** to solve this in a single pass without knowing the list length beforehand. The idea is:

1. Create a `dummy` node pointing to `head` to handle edge cases (like deleting the head itself).
2. Move `fast` pointer `n+1` steps ahead from the dummy.
3. Move both `fast` and `slow` together until `fast` reaches `null`.
4. Now `slow` is exactly one step before the target node.
5. Do `slow->next = slow->next->next` to skip (delete) the target node.

The gap of `n+1` (instead of `n`) ensures `slow` lands **just before** the node to delete, making removal easy.

---

## Code

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        ListNode* fast = dummy;
        ListNode* slow = dummy;

        // Move fast n+1 steps ahead
        for (int i = 0; i <= n; i++) {
            fast = fast->next;
        }

        // Move both until fast hits null
        while (fast != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }

        // Delete the nth node from end
        ListNode* toDelete = slow->next;
        slow->next = slow->next->next;
        delete toDelete;

        return dummy->next;
    }
};
```

---

## Complexity

**Time** :O(sz)
**Space** :O(1)

