# 26. Remove Duplicates from Sorted Array

**Author:** Chhavi
**Platform:** LeetCode
**Difficulty:** Easy
**Language:** C++

---

## Problem

We're given a sorted array and we have to remove duplicates in-place and return how many unique elements are there.

---

## My Approach

Since the array is already sorted, duplicates will always be next to each other. So I just compared each element with the previous one — if they're different, it's a new unique element and I placed it at position `k`.

Basically a two pointer idea where `k` keeps track of where to put the next unique value.

---

## Code
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int k = 1;
        for(int i = 1; i < nums.size(); i++){
            if(nums[i] != nums[i-1]){
                nums[k] = nums[i];
                k++;
            }
        }
        return k;
    }
};
```

---

## Complexity

- Time: O(n)
- Space: O(1)
