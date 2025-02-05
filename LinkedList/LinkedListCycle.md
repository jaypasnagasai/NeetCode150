# 141. Linked List Cycle
Difficulty: Easy

## QUESTION

Given the beginning of a linked list `head`, return `true` if there is a cycle in the linked list. Otherwise, return `false`.

There is a cycle in a linked list if at least one node in the list that can be visited again by following the `next` pointer.

Internally, `index` determines the index of the beginning of the cycle, if it exists. The tail node of the list will set it's `next` pointer to the `index-th` node. If index = -1, then the tail node points to `null` and no cycle exists.

Note: `index` is not given to you as a parameter.

### EXAMPLE

```
Input: head = [1,2,3,4], index = 1
Output: true
```

```
Input: head = [1,2], index = -1
Output: false
```

## SOLUTION


### APPROACH: HASH SET

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Importing Optional for type hinting

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        """
        Detects if a linked list has a cycle using a hash set.
        
        :param head: The head node of the linked list.
        :return: True if a cycle is detected, otherwise False.
        """
        seen = set()  # Set to store visited nodes
        cur = head  # Pointer to traverse the linked list
        
        while cur:  # Iterate through the linked list
            if cur in seen:  # If the current node is already seen, a cycle exists
                return True
            seen.add(cur)  # Mark the current node as visited
            cur = cur.next  # Move to the next node
        
        return False  # If the loop exits, no cycle was detected
```

### APPROACH: FAST AND SLOW POINTERS

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Importing Optional for type hinting

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        """
        Detects if a linked list has a cycle using Floyd's Cycle Detection Algorithm (Tortoise and Hare Algorithm).
        
        :param head: The head node of the linked list.
        :return: True if a cycle is detected, otherwise False.
        """
        slow, fast = head, head  # Initialize two pointers, both starting at the head
        
        while fast and fast.next:  # Ensure fast pointer and its next node exist
            slow = slow.next  # Move slow pointer one step
            fast = fast.next.next  # Move fast pointer two steps
            
            if slow == fast:  # If they meet, a cycle is present
                return True
        
        return False  # If fast reaches the end, there is no cycle
```
