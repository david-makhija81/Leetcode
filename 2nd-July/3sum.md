# Problem Statement
Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

 

Example 1:

    Input: nums = [-1,0,1,2,-1,-4]
    Output: [[-1,-1,2],[-1,0,1]]
    Explanation: 
    nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
    nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
    nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
    The distinct triplets are [-1,0,1] and [-1,-1,2].
    Notice that the order of the output and the order of the triplets does not matter.

Example 2:

    Input: nums = [0,1,1]
    Output: []
    Explanation: The only possible triplet does not sum up to 0.

Example 3:

    Input: nums = [0,0,0]
    Output: [[0,0,0]]
    Explanation: The only possible triplet sums up to 0.
 

Constraints:

    3 <= nums.length <= 3000
    -10^5 <= nums[i] <= 10^5

# Solution 1
## Intuition
We need to find 3 elements such that none of these 3 chosen elements share the same position mutually and their sum equals zero. We can use induction to atomize the problem further like: For each element nums[i] we have to find 2 other elements nums[j], nums[k] on different positions that add up to the negative of the first element and when we have exhaustively searched for all the potential pairs of two elements that add up to the negative of the first element, we should no longer consider that first element nums[i] in our further searches because we have looked at all the cases exhaustively where it is a part of any triplet whatsoever. Now, to find these 2 elements further atomizing the problem via induction: For each element nums[j] find an element nums[k] that is equal to the negative of sum of this element nums[j] and the previous carry nums[i].

## Approach
1. For each element nums[i] we look for 2 elements that sum up to the negative of this element. And we will not include this element nums[i] further in our **triplet** searches as discussed in the Intuition.
2. To find these 2 elements nums[j], nums[k] we atomize this problem further by something like: For each element nums[j] we in the possible search space, we find a nums[k] that adds up to the negative sum of the previous 2 elements (-(nums[i] + nums[j])). And we will not include this element nums[j] further in our **doublet** searches because of the same reason stated earlier.
3. We keep a record of triplets that satisfy our condition and then return this collection of triplets.

## Code

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;

        for(int i = 0; i < nums.size(); i++) {
            for(int j = i + 1; j < nums.size(); j++) {
                for(int k = j + 1; k < nums.size(); k++) {
                    if((nums[i] + nums[j] + nums[k]) == 0) {
                        result.push_back({nums[i], nums[j], nums[k]});
                    }
                }
            }
        }

        return result;
    }
};
```

## Complexity

**Time Complexity: O(N<sup>3</sup>)**

**Space Complexity: O(1)**

*Where:* 
- **N** is the number if elements in the array nums.
