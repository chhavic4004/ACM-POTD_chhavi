# 42. Trapping Rain Water
**Author:** Chhavi
**Platform:** LeetCode
**Difficulty:** Hard
**Language:** C++

---

## Problem
Given n non-negative integers representing an elevation map, compute how much water it can trap after raining.

---

## My Approach
Water trapped at any position depends on the minimum of the tallest bar to its left and right. Instead of precomputing two separate arrays for that, I used two pointers moving inward from both ends.

Whichever side has the smaller height, that side's max is the bottleneck — so I calculate the water there and move that pointer inward. This way I never need extra space.

---

## Code
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int left = 0, right = height.size() - 1;
        int maxLeft = 0, maxRight = 0;
        int water = 0;

        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= maxLeft)
                    maxLeft = height[left];
                else
                    water += maxLeft - height[left];
                left++;
            } else {
                if (height[right] >= maxRight)
                    maxRight = height[right];
                else
                    water += maxRight - height[right];
                right--;
            }
        }

        return water;
    }
};
```

---

## Complexity
- Time: O(n)
- Space: O(1)
