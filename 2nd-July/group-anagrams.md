# Problem Statement
Given an array of strings strs, group the anagrams together. You can return the answer in any order.

Example 1:

    Input: strs = ["eat","tea","tan","ate","nat","bat"]

    Output: [["bat"],["nat","tan"],["ate","eat","tea"]]

    Explanation:

    There is no string in strs that can be rearranged to form "bat".

    The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.

    The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.

Example 2:

    Input: strs = [""]

    Output: [[""]]

Example 3:

    Input: strs = ["a"]

    Output: [["a"]]

 

Constraints:

    1 <= strs.length <= 10^4

    0 <= strs[i].length <= 100

    strs[i] consists of lowercase English letters.

# Intuition
By definition, anagram of a word gets created when characters of that word are reordered in a different arrangement other than what it was earlier. Thus, two anagrams must have the same set of characters irrespective of the order they are arranged in. Thus, if we just compared those set of characters and not explicitly the order they are arranged in, it would aid us to identify if two words are anagrams to each other. Also, if we sort 2 arrangements having the same set of characters but arranged in 2 different orders both will eventually result in the same arrangement. Thus, we can then compare these sorted versions of sequences to figure out if they are anagrams or not.

# Approach
1. Make a hashmap that stores vectors of strings against string keys.
2. For each string, determine it's "key sequence". A "key sequence" of a string is it's sorted version, because it helps us identify that string's anagrams.
3. If that keySequence already exists in the hashmap, we push the current string into the vector mapped against this keySequence.
4. Else, we insert an empty vector against this keySequence and then push the string into that vector.
5. At the end, we return all the vectors in the hashmap.

# Code

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> anagramGroup;

        for(string str: strs) {
            string keySequence = str;
            sort(keySequence.begin(), keySequence.end());

            if(anagramGroup.find(keySequence) == anagramGroup.end()) {
                anagramGroup.insert({keySequence, {}});
            }

            anagramGroup[keySequence].push_back(str);
        }

        vector<vector<string>> result;

        for(auto itr = anagramGroup.begin(); itr != anagramGroup.end(); itr++) {
            result.push_back(itr -> second);
        }

        return result;
    }
};
```

# Complexity

**Time Complexity: O(N * M * log(M))** (Worst Case)

**Space Complexity: O(N)**

*Where:* 
- **N** is the Number of strings in the vector.
- **M** is the Max number of characters in a string.
