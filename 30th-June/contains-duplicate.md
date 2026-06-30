# Problem Statement
Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

 

Example 1:

    Input: nums = [1,2,3,1]
    Output: true
    Explanation: The element 1 occurs at the indices 0 and 3.

Example 2:

    Input: nums = [1,2,3,4]
    Output: false
    Explanation: All elements are distinct.

Example 3:

    Input: nums = [1,1,1,3,3,4,3,2,4,2]
    Output: true

 

Constraints:

    1 <= nums.length <= 10^5
    -10^9 <= nums[i] <= 10^9
<br/>
<br/>
<br/>
<br/>

# Solution 1

## Intuition
The Task is to spot if any element in the array exists more than. When an arrangement with duplicate elements is rearranged in a non-ascending order the duplicate elements come consecutive to each other.

## Algorithm
1. Sort the array of numbers in a non-ascending order. 
*(Now the duplicate elements must have come consecutive to each other)*
2. Go through the arrangement of elements comparing each element to the one placed next to it, if the 2 elements match - ***return true***.
3. If after going through all the elements you do not find any duplicate neighbours - ***return false***.

## Code

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        for(int i = 0; i < (nums.size() - 1); i++) {
            if(nums[i] == nums[i + 1]) {
                return true;
            }
        }

        return false;
    }
};
```

## Complexity

**Time Complexity: O(N*log(N))**

**Space Complexity: O(1)**

*Where:* 
- **N** is the number of elements in the arrangement.
<br/>
<br/>
<br/>
<br/>

# Solution 2
The previous solution alters the original arrangement. Alternatively let's try a solution that keeps the original arrangement in tact.

## Intuition
If we maintain the count of each element in a separate structure, it is easy to spot if any element occurred more than once.

## Algorithm
1. Create a hashmap that maps integer keys to integer values.
*(It will be useful for keeping track of the count of each element)*

2. Go through the arrangement - each element at a time - if the element already exists in the hashmap, break the loop and - ***return true***.

3. If the element is not yet present in the hashmap, add it to the hashmap as a key and set the value against this key to be equal to 1 denoting the number of times it has occurred till now.

4. If after going through elements you do not come across an element that was already present in the hashmap - ***return false***.

## Code

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> alreadyOccuredNums;

        for(int num: nums) {
            if(alreadyOccuredNums.find(num) != alreadyOccuredNums.end()) {
                return true; // Current element already exists in the set
            }

            alreadyOccuredNums.insert(num);
        }

        return false;
    }
};
```

## Complexity

**Average Time Complexity: O(N)**

**Worst Time Complexity: O(N<sup>2</sup>)**

**Space Complexity: O(N)**

*Where:* 
- **N** is the number of elements in the arrangement.
