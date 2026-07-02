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

# Solution 2
Talking about the previous solution, it took way more time for the test cases then it is allowed to. Also, it did not filter out duplicate triplets. Thus, this solution is an attempt to reduce the time complexity as well as covering the edge cases of filtering out the duplicate triplets.

## Intuition
To reduce the time complexity we can introduce some changes in the part where we are searching for two elements that sum up to the negative of the first element. If we look at the problem statement it says that the resulting triplets can be arranged in any order whatsoever. Thus, we may rearrange the sequence in any order that aids us in finding these triplets in a much faster way. If we rearrange this sequence in a non-descending order, then the number of operations required for finding a sum of two elements becomes way less than what it was earlier. And if we eliminate the duplicate elements from the sequence it further eliminates the risk of adding duplicate triplets. But there will be some edge cases where we will need the duplicate elements as they can also contribute in the desired results like: {-1, -1, 2}, {1, 1, -2} and {0, 0, 0}.

## Approach
1. First we will sort the sequence in a non-descending order.
1. Then we will compute the special triplets that contain duplicates and that are only made via duplicates.
2. Then we will make a new sequence that does not consist of duplicate elements.
3. Then for each element, we take out the rest of the sequence grab the 2 ends of that sequence opposite to each other.
4. We will shift the observations on the ends towards each other until they meet in the middle.
5. If the sum of the elements on these 2 ends is greater than the required sum we shift the right end towards left because the sequence is non-descending and thus, the right element is the bigger one of the two and if we shift our observation to one position left of the right end then the new sum would be smaller than the previous one.
6. Else if the sum is greater than required we consider the element on one position to the right of the left end.
7. When we find the 2 elements with their sum equal to the desired value, we add to the record.

## Code

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;

        sort(nums.begin(), nums.end());
        int zeroCount = 0;

        for(int i = 0; i < nums.size() - 1; i++) {
            if((nums[i] == nums[i + 1]) && ((i == 0) || (nums[i] != nums[i - 1]))) {
                for(int j = i + 2; j < nums.size(); j++) {
                    if((nums[j] != nums[i]) && ((nums[i] + nums[i + 1] + nums[j]) == 0)) {
                        result.push_back({nums[i], nums[i + 1], nums[j]});
                        break;
                    }
                }
            }

            if(nums[i] == 0) {
                zeroCount++;
            }
        }

        zeroCount = (nums[nums.size() - 1] == 0) ? (zeroCount + 1) : zeroCount;

        if(zeroCount > 2) {
            result.push_back({0, 0, 0});
        }

        for(int i = nums.size() - 1; i > 0; i--) {
            if((nums[i] == nums[i - 1]) && ((i == (nums.size() - 1)) || (nums[i] != nums[i + 1]))) {
                for(int j = i - 2; j >= 0; j--) {
                    if((nums[j] != nums[i]) && ((nums[j] + nums[i - 1] + nums[i]) == 0)) {
                        result.push_back({nums[j], nums[i - 1], nums[i]});
                        break;
                    }
                }
            }
        }

        vector<int> noDuplicates;

        for(int num: nums) {
            if(noDuplicates.empty() || (noDuplicates.back() != num)) {
                noDuplicates.push_back(num);
            }
        }

        for(int i = 0; i < noDuplicates.size(); i++) {
            int leftEnd = i + 1, rightEnd = (noDuplicates.size() - 1);

            while(leftEnd < rightEnd) {
                int sum = (noDuplicates[i] + noDuplicates[leftEnd] + noDuplicates[rightEnd]);
                if(sum == 0) {
                    result.push_back({noDuplicates[i], noDuplicates[leftEnd], noDuplicates[rightEnd]});
                    leftEnd++;
                    rightEnd--;
                } else if(sum < 0) {
                    leftEnd++;
                } else {
                    rightEnd--;
                }
            }
        }

        return result;
    }
};
```

## Complexity

**Time Complexity: O(N<sup>2</sup>)**

**Space Complexity: O(M)**

*Where:* 
- **N** is the number of elements in the sequence. 
- **M** is the number of unique elements in the sequence.
