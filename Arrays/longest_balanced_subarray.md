# Longest Balanced Subarray

## Problem
Given an integer array `nums`, a subarray is **balanced** when:

- the number of **distinct even** values in the subarray
- equals the number of **distinct odd** values in the subarray.

Return the length of the longest balanced subarray.

## Key idea
For a fixed right endpoint `r`, move the left endpoint `l` backward while tracking:

- frequency of each even value in `nums[l..r]`
- frequency of each odd value in `nums[l..r]`
- `distinctEven` and `distinctOdd`

Whenever `distinctEven == distinctOdd`, update the answer with `r - l + 1`.

This checks all subarrays once, so time complexity is `O(n^2)` and space is `O(n)` for the maps.

## C++ reference solution
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int longestBalancedSubarray(vector<int>& nums) {
        int n = (int)nums.size();
        int ans = 0;

        for (int r = 0; r < n; ++r) {
            unordered_map<int, int> evenFreq;
            unordered_map<int, int> oddFreq;
            int distinctEven = 0;
            int distinctOdd = 0;

            for (int l = r; l >= 0; --l) {
                int x = nums[l];

                if (x % 2 == 0) {
                    if (evenFreq[x] == 0) {
                        ++distinctEven;
                    }
                    ++evenFreq[x];
                } else {
                    if (oddFreq[x] == 0) {
                        ++distinctOdd;
                    }
                    ++oddFreq[x];
                }

                if (distinctEven == distinctOdd) {
                    ans = max(ans, r - l + 1);
                }
            }
        }

        return ans;
    }
};
```

## Walkthrough
For `nums = [2, 1, 2, 3]`:

- Subarray `[2, 1]` has distinct evens `{2}` and odds `{1}` → balanced (length 2)
- Subarray `[2, 1, 2, 3]` has distinct evens `{2}` and odds `{1, 3}` → not balanced
- Subarray `[1, 2, 3]` has distinct evens `{2}` and odds `{1, 3}` → not balanced

Best answer is `3` (from `[2, 1, 2]`).

## If you want to optimize
If constraints are very large (for example `n` up to `2e5`), share the limits and we can discuss faster ideas.
