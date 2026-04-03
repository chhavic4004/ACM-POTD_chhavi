# LC 33. Search in Rotated Sorted Array

**Difficulty:** Medium  
**Topic:** Binary Search  
**Author:** Chhavi

---

## Problem Statement

There is an integer array `nums` sorted in ascending order with distinct values. It may be rotated at an unknown index.

Given the array and a target value, return the index of the target if it exists, otherwise return `-1`.

You must achieve **O(log n)** time complexity.

**Constraints:**
- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- All values are unique
- Array is sorted and rotated
- `-10^4 <= target <= 10^4`

---

## Banned Solution

> Standard binary search that directly checks which half is sorted and adjusts boundaries accordingly.

---

## Approach — Find Pivot + Binary Search

### Intuition

A rotated sorted array consists of **two sorted parts**:
- Left part (before rotation)
- Right part (after rotation)

The smallest element is the **pivot point**.

If we find this pivot:
- We can determine which half the target belongs to
- Then apply normal binary search on that half

---

### Key Insight

The pivot is the index where:
- `nums[i] > nums[i+1]`

Or more efficiently:
- The smallest element can be found using binary search

---

### Steps

1. Find the pivot (index of smallest element).
2. Decide search range:
   - If target is between `nums[pivot]` and `nums[n-1]` → search right side
   - Else → search left side
3. Perform standard binary search in chosen range.

---

## Code

```cpp
class Solution {
public:
    int findPivot(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        return left; // index of smallest element
    }

    int binarySearch(vector<int>& nums, int left, int right, int target) {
        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] == target) return mid;
            else if (nums[mid] < target) left = mid + 1;
            else right = mid - 1;
        }
        return -1;
    }

    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int pivot = findPivot(nums);

        if (target >= nums[pivot] && target <= nums[n - 1]) {
            return binarySearch(nums, pivot, n - 1, target);
        } else {
            return binarySearch(nums, 0, pivot - 1, target);
        }
    }
};


![screenshot](chhavichoudhary_3-4-2026-day13-hard.png)