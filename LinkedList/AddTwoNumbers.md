# 2. Add Two Numbers
Difficulty: Medium

## QUESTION

You are given two non-empty linked lists, `l1` and `l2`, where each represents a non-negative integer.

The digits are stored in reverse order, e.g. the number 123 is represented as `3 -> 2 -> 1 ->` in the linked list.

Each of the nodes contains a single digit. You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Return the sum of the two numbers as a linked list.

### EXAMPLE

```
Input: l1 = [1,2,3], l2 = [4,5,6]
Output: [5,7,9]
Explanation: 321 + 654 = 975.
```

```
Input: l1 = [9], l2 = [9]
Output: [8,1]
```

## SOLUTION


### APPROACH: RECURSION

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Importing Optional for type hinting

class Solution:
    def add(self, l1: Optional[ListNode], l2: Optional[ListNode], carry: int) -> Optional[ListNode]:
        """
        Recursively adds two numbers represented as linked lists, digit by digit.

        :param l1: First linked list representing a non-negative integer (in reverse order).
        :param l2: Second linked list representing a non-negative integer (in reverse order).
        :param carry: Carry from the previous digit addition.
        :return: The head of the newly formed linked list representing the sum.
        """
        
        # Base case: If both lists are empty and no carry remains, return None
        if not l1 and not l2 and carry == 0:
            return None
        
        # Extract values from current nodes (if present)
        v1 = l1.val if l1 else 0
        v2 = l2.val if l2 else 0
        
        # Compute sum and determine new carry
        carry, val = divmod(v1 + v2 + carry, 10)  # (sum // 10, sum % 10)
        
        # Recursive call to process next digits and carry
        next_node = self.add(
            l1.next if l1 else None,  # Move to the next node in l1 (if exists)
            l2.next if l2 else None,  # Move to the next node in l2 (if exists)
            carry  # Pass the carry to the next recursive call
        )
        
        # Return the new node containing the current sum digit
        return ListNode(val, next_node)

    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        """
        Wrapper function to initiate recursive addition of two numbers.

        :param l1: First linked list (non-negative integer in reverse order).
        :param l2: Second linked list (non-negative integer in reverse order).
        :return: The head of the new linked list representing the sum.
        """
        return self.add(l1, l2, 0)  # Start recursive addition with initial carry of 0
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
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        """
        Adds two numbers represented as linked lists, where each node contains a single digit.
        The numbers are stored in reverse order, meaning that the 1's place is the head of the list.

        This implementation follows an **iterative approach** using a dummy node and pointer.

        :param l1: First linked list representing a non-negative integer (in reverse order).
        :param l2: Second linked list representing a non-negative integer (in reverse order).
        :return: The head of the newly formed linked list representing the sum.
        """
        
        dummy = ListNode()  # Dummy node to simplify handling the head of the result list
        cur = dummy  # Pointer to build the new linked list

        carry = 0  # Variable to keep track of carry-over during addition

        # Iterate through both lists until all digits are processed
        while l1 or l2 or carry:
            # Extract values from current nodes (default to 0 if node is None)
            v1 = l1.val if l1 else 0
            v2 = l2.val if l2 else 0

            # Compute new digit value and carry
            val = v1 + v2 + carry
            carry = val // 10  # Carry is the tens place of the sum
            val = val % 10  # Store only the units place in the new node

            # Create new node with computed value
            cur.next = ListNode(val)

            # Move pointers forward
            cur = cur.next
            l1 = l1.next if l1 else None  # Move to next node in l1 (if exists)
            l2 = l2.next if l2 else None  # Move to next node in l2 (if exists)

        # Return the result list (excluding the dummy node)
        return dummy.next
```
