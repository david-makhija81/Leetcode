# Problem Statement
Given the head of a singly linked list, reverse the list, and return the reversed list.

 

Example 1:

![alt text](../example-images/reverse-linked-list-example1.jpg)

    Input: head = [1,2,3,4,5]
    Output: [5,4,3,2,1]

Example 2:

![alt text](../example-images/reverse-linked-list-example2.jpg)

    Input: head = [1,2]
    Output: [2,1]
Example 3:

    Input: head = []
    Output: []
 

Constraints:

    The number of nodes in the list is the range [0, 5000].
    -5000 <= Node.val <= 5000
 



# Intuition
Starting from the last node we need to make each node point to the previous element. However, a node can only access the next element and cannot access the previous element. Thus, the previous element must be stored somewhere.

# Approach
1. Create an arrangement VisitedNodes to store all the nodes.
2. Visit every node via the previous one's next pointer.
3. As you go through the nodes keep storing them in the VisitedNodes.
4. After storing all the nodes, Start from the rear end of the VisitedNodes and keep updating their pointers.

# Code

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == NULL) {
            return NULL;
        }

        vector<ListNode*> visitedNodes;

        for(int i = 0; head != NULL; i++) {
            visitedNodes.push_back(head);

            head = head -> next;
        }

        for(int i = (visitedNodes.size() - 1); i > 0; i--) {
            visitedNodes[i] -> next = visitedNodes[i - 1];
        }

        visitedNodes[0] -> next = NULL;

        return visitedNodes[visitedNodes.size() - 1];
    }
};
```

# Complexity

**Time Complexity: O(N)**

**Space Complexity: O(N)**

*Where:* 
- **N** is the number of nodes in the Linked List
