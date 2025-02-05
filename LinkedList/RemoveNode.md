# 19. Remove Nth Node From End of List
Difficulty: Medium

## QUESTION

You are given the beginning of a linked list `head`, and an integer `n`.

Remove the `nth` node from the end of the list and return the beginning of the list.

### EXAMPLE

```
Input: head = [1,2,3,4], n = 2
Output: [1,2,4]
```

```
Input: head = [5], n = 1
Output: []
```

```
Input: head = [1,2], n = 2
Output: [2]
```
## SOLUTION


### APPROACH: BRUTE FORCE

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Importing Optional for type hinting

class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        """
        Removes the nth node from the end of the linked list.

        This implementation stores all nodes in an array, determines the
        target node to remove, and adjusts the links accordingly.

        :param head: The head of the linked list.
        :param n: The position of the node from the end to remove.
        :return: The head of the modified linked list.
        """

        # Step 1: Store all nodes in an array
        nodes = []
        cur = head
        while cur:
            nodes.append(cur)  # Append each node to the list
            cur = cur.next  # Move to the next node
        
        # Step 2: Find the index of the node to be removed
        removeIndex = len(nodes) - n  # Convert from end-position to index
        
        # Step 3: Handle special case where the head needs to be removed
        if removeIndex == 0:
            return head.next  # Return the second node as the new head
        
        # Step 4: Remove the target node by updating the previous node's pointer
        nodes[removeIndex - 1].next = nodes[removeIndex].next  # Skip the target node
        
        return head  # Return the updated head of the list
```

### APPROACH: ITERATION

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Importing Optional for type hinting

class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        """
        Removes the nth node from the end of the linked list.

        This implementation follows a two-pass approach:
        1. Determine the total length (N) of the linked list.
        2. Calculate the position from the start (N - n) and remove the node.

        :param head: The head of the linked list.
        :param n: The position of the node from the end to remove.
        :return: The head of the modified linked list.
        """

        # Step 1: Compute the total length of the linked list
        N = 0  # Length counter
        cur = head
        while cur:
            N += 1
            cur = cur.next  # Traverse the list to count nodes
        
        # Step 2: Determine the index of the node to be removed
        removeIndex = N - n  # Convert position from end to start index

        # Step 3: Handle special case where the head needs to be removed
        if removeIndex == 0:
            return head.next  # Return the second node as the new head
        
        # Step 4: Traverse again to remove the target node
        cur = head
        for i in range(N - 1):  # Traverse the list
            if (i + 1) == removeIndex:  # When we reach the node before the target
                cur.next = cur.next.next  # Skip the target node
                break
            cur = cur.next  # Move to the next node

        return head  # Return the updated head of the list
```

### APPROACH: RECURSION

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Importing Optional for type hinting

class Solution:
    def rec(self, head, n):
        """
        Recursively traverses to the end of the list, decrementing n as it backtracks.
        When n reaches 0, it removes the target node by skipping it.

        :param head: The current node in recursion.
        :param n: A list containing the value of n (passed as a reference).
        :return: The modified linked list after removing the nth node from the end.
        """
        if not head:  # Base case: If we reach the end, return None
            return None

        # Recursively traverse to the end of the list
        head.next = self.rec(head.next, n)

        # Decrement n[0] on the way back (backtracking)
        n[0] -= 1

        # When n[0] reaches 0, remove the target node by skipping it
        if n[0] == 0:
            return head.next  # Bypass the current node

        return head  # Return the current node (unchanged)
    
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        """
        Removes the nth node from the end of the linked list using recursion.

        This function calls a helper recursive function (`rec`) with a mutable list `[n]`
        so that `n` can be decremented during backtracking.

        :param head: The head of the linked list.
        :param n: The position from the end of the node to be removed.
        :return: The head of the modified linked list.
        """
        return self.rec(head, [n])  # Pass `n` as a mutable list to track decrements
```

### APPROACH: TWO POINTERS

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Importing Optional for type hinting

class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        """
        Removes the nth node from the end of the linked list using the two-pointer approach.

        This implementation ensures O(1) space complexity and a single-pass traversal.

        :param head: The head of the linked list.
        :param n: The position from the end of the node to be removed.
        :return: The head of the modified linked list.
        """

        # Step 1: Create a dummy node to handle edge cases (e.g., removing the head)
        dummy = ListNode(0, head)
        left = dummy  # Left pointer starts from dummy
        right = head  # Right pointer starts from the head

        # Step 2: Move the right pointer `n` steps ahead
        while n > 0:
            right = right.next
            n -= 1  # Decrement n at each step

        # Step 3: Move both left and right pointers together until right reaches the end
        while right:
            left = left.next
            right = right.next  # Move both pointers at the same pace

        # Step 4: Remove the nth node from the end
        left.next = left.next.next  # Skip the target node

        # Step 5: Return the modified list (dummy.next handles cases where head is removed)
        return dummy.next
```
