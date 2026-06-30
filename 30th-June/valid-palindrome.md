# Problem Statement
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

 

Example 1:

    Input: s = "A man, a plan, a canal: Panama"
    Output: true
    Explanation: "amanaplanacanalpanama" is a palindrome.
Example 2:

    Input: s = "race a car"
    Output: false
    Explanation: "raceacar" is not a palindrome.
Example 3:

    Input: s = " "
    Output: true
    Explanation: s is an empty string "" after removing non-alphanumeric characters.
    Since an empty string reads the same forward and backward, it is a palindrome.
 

Constraints:

    1 <= s.length <= 2 * 10^5
    s consists only of printable ASCII characters.
 

# Intuition
To identify if an arrangement is a palindrome or not, one must hold the 2 ends of the arrangement comparing the 2 ends with one another advancing both observations towards each other until it reaches the middle. This Solution will be following this method with slight variations - such as skipping the nonalphanumeric characters on either side and a separate component that compares denominations of alphabets regardless of them being LowerCase or UpperCase.

# Approach
1. Make a Component that tells if a character is an alphabet regardless of it being lowerCase or upperCase.

2. Make another Component that compares if 2 alphabets are of same denomination or not regardless if they are lowerCase or upperCase.

3. Start from each end of string, if either one of those ends is not alpha-numeric skip the character at that end and move that pointer to the middle.

4. If the characters at both the ends are alphanumeric, Check if one is alphabet and the other one is not then - ***return false***.

5. If characters at both the ends are either numbers or alphabets compare the characters to each other, if they numbers compare them raw else use the component we made earlier for comparing alphabets. 

6. If the characters at the 2 ends do not match - ***return false***.

7. If both the observations reach the middle that means there were no mismatches thus - ***return true***.

# Code

```cpp
class Solution {

    bool isAlphabet(char c) {

        bool isLowerCase = (((c - 'a') >= 0) && ((c - 'a') < 26));
        bool isUpperCase = (((c - 'A') >= 0) && ((c - 'A') < 26));

        return (isLowerCase || isUpperCase);
    }

    bool isNumber(char c) {
        return (((c - '0') >= 0) && ((c - '0') < 10));
    }

    bool isAlphaNumeric(char c) {
        return (isAlphabet(c) || isNumber(c));
    }

    bool alphabetCompare(char alphabet1, char alphabet2) {
        int alphabet1Denomination = -1, alphabet2Denomination = -1;

        if(((alphabet1 - 'a') >= 0) && ((alphabet1 - 'a') < 26)) {
            alphabet1Denomination = (alphabet1 - 'a');
        } else {
            alphabet1Denomination = (alphabet1 - 'A');
        }

        if(((alphabet2 - 'a') >= 0) && ((alphabet2 - 'a') < 26)) {
            alphabet2Denomination = (alphabet2 - 'a');
        } else {
            alphabet2Denomination = (alphabet2 - 'A');
        }

        return (alphabet2Denomination == alphabet1Denomination);
    }

public:
    bool isPalindrome(string s) {
        int leftEnd = 0, rightEnd = s.size() - 1;

        while(leftEnd < rightEnd) {
            while((leftEnd < rightEnd) && !isAlphaNumeric(s[leftEnd])) {
                leftEnd++;
            }
            while((leftEnd < rightEnd) && !isAlphaNumeric(s[rightEnd])) {
                rightEnd--;
            }

            if(leftEnd >= rightEnd) {
                break;
            }

            if(isAlphabet(s[leftEnd]) && isAlphabet(s[rightEnd])) {
                if(!alphabetCompare(s[leftEnd], s[rightEnd])) {
                    return false;
                }
            } else if((!isAlphabet(s[leftEnd])) && (!isAlphabet(s[rightEnd]))) {
                if(s[leftEnd] != s[rightEnd]) {
                    return false;
                }
            } else {
                return false; // one character is and the other one is a number.
            }

            leftEnd++;
            rightEnd--;
        }

        return true; // No mismatch detected.
    }
};
```

# Complexity

**Time Complexity: O(N)**

**Space Complexity: O(1)**

*Where:* 
- **N** is the number of characters in the string.
