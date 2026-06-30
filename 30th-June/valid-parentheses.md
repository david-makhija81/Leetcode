# Problem Statement
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:

    Input: s = "()"

    Output: true

Example 2:

    Input: s = "()[]{}"

    Output: true

Example 3:

    Input: s = "\(\]"

    Output: false

Example 4:

    Input: s = "([])"

    Output: true

Example 5:

    Input: s = "([)]"

    Output: false

 

Constraints:

    1 <= s.length <= 10^4
    s consists of parentheses only '()[]{}'.

# Intuition
If I consider a couple of Examples:
    
    "{[}]" // This expression is invalid
    "{[]}" // This expression is valid

It is pretty clear that for an expression of brackets to be valid if it consists of a particular sequence of opening brackets *" { ( [ "* - the last bracket of this sequence must be the first one to be closed to make room for other brackets to close that occurred before it i.e. for the sequence in discussion - first [ has to close, only then does it make room for ( to be closed and so on.

# Approach
*The Intuition strongly suggests towards using a First-In-First-Out kind of data structure*

1. We maintain a stack and will only push opening brackets in it.
2. We pop out the bracket on top only if we find a closing bracket corresponding to opening bracket on top of the stack.
3. If we encounter a closing that does not match with the opening bracket on top of the stack. we ***return false***.
4. Else if we encounter a closing bracket and the stack is empty then we ***return false***.
4. If at the end of going through the entire sequence the stack is still filled. we ***return false***.
6. Else we ***return true***.

# Code

```cpp
class Solution {

    bool isOpeningBracket(char bracket) {
        return ((bracket == '{') || (bracket == '[') || (bracket == '('));
    }

    char correspondingClosingBracket(char bracket) {
        return ((bracket == '{') ? '}' : ((bracket == '[') ? ']' : ')'));
    } 

public:
    bool isValid(string s) {
        stack<char> openingBrackets;

        for(char bracket: s) {
            if(isOpeningBracket(bracket)) {
                openingBrackets.push(bracket);
            } else if(
                        !openingBrackets.empty() && 
                        (bracket == correspondingClosingBracket(openingBrackets.top()))
                    ) {
                openingBrackets.pop();
            } else {
                return false;
            }
        }

        return openingBrackets.empty(); // All opened brackets must be closed.
    }
};
```

# Complexity

**Time Complexity: O(N)**

**Space Complexity: O(N)**

*Where:* 
- **N** is the number of characters in string s.
