# 206. Reverse Linked List
Difficulty: Easy

## QUESTION

Given the beginning of a singly linked list `head`, reverse the list, and return the new beginning of the list.

### EXAMPLE

```
Input: head = [0,1,2,3]
Output: [3,2,1,0]
```

```
Input: head = []
Output: []
```

## SOLUTION


### APPROACH: RECURSION

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Base case: if the list is empty, return None
        if not head:
            return None

        # Assume the new head is the current head initially
        newHead = head

        # Recursive case: if there's a next node, reverse the rest of the list
        if head.next:
            newHead = self.reverseList(head.next)  # Recursively reverse the rest of the list
            head.next.next = head  # Reverse the pointer direction

        # Ensure the original head's next pointer is set to None (to avoid cycles)
        head.next = None

        return newHead  # Return the new head of the reversed list
```

### APPROACH: ITERATION

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev, curr = None, head  # Initialize previous node as None and current node as head

        while curr:
            temp = curr.next  # Store the next node
            curr.next = prev  # Reverse the current node's pointer
            prev = curr  # Move prev to the current node
            curr = temp  # Move to the next node
        
        return prev  # Return the new head (last node in original list)
```
