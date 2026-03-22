# 15. 3Sum
**Author:** Chhavi
**Platform:** LeetCode
**Difficulty:** Medium
**Language:** C++

---

## Problem
Given an integer array, find all unique triplets that add up to zero.

---

## My Approach
First I sorted the array — that way duplicates sit next to each other and I can skip them easily. Then I fixed one element at a time with index `i` and used two pointers (`left` and `right`) on the rest of the array to find pairs that sum to `-nums[i]`.

If the sum is too small, move `left` right. If too big, move `right` left. Whenever a valid triplet is found, skip over any duplicate values on both sides before continuing.

---

## Code
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        int n = nums.size();

        for (int i = 0; i < n - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int left = i + 1, right = n - 1;

            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];

                if (sum == 0) {
                    result.push_back({nums[i], nums[left], nums[right]});
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    while (left < right && nums[right] == nums[right - 1]) right--;
                    left++;
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    right--;
                }
            }
        }

        return result;
    }
};
```

---

## Complexity
- Time: O(n²)
- Space: O(1)
