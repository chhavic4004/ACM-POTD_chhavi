1346. Check If N and Its Double Exist
Author: Chhavi
Platform: LeetCode
Difficulty: Easy
Language: C++

Problem
We're given an array of integers and we have to check if there exist two different indices i and j such that arr[i] == 2 * arr[j].

My Approach
I used a HashSet to store elements as I traverse the array. For each number, I check two things before inserting it:

Is 2 * num already in the set? (current is the half, previous was the double)
Is num / 2 already in the set AND num is even? (current is the double, previous was the half)

If either condition is true, we found our pair. This way we handle both directions in a single pass without needing nested loops.

Code
cppclass Solution {
public:
    bool checkIfExist(vector<int>& arr) {
        unordered_set<int> seen;
        for (int num : arr) {
            if (seen.count(2 * num) || (num % 2 == 0 && seen.count(num / 2))) {
                return true;
            }
            seen.insert(num);
        }
        return false;
    }
};

Complexity

Time: O(n)
Space: O(n)
