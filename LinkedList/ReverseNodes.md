# 25. Reverse Nodes In K-Group
Difficulty: Hard

## QUESTION

You are given the head of a singly linked list `head` and a positive integer `k`.

You must reverse the first `k` nodes in the linked list, and then reverse the next `k` nodes, and so on. If there are fewer than `k` nodes left, leave the nodes as they are.

Return the modified list after reversing the nodes in each group of `k`.

You are only allowed to modify the nodes' `next` pointers, not the values of the nodes.

### EXAMPLE

```
Input: head = [1,2,3,4,5,6], k = 3
Output: [3,2,1,6,5,4]
```

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
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
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        """
        Reverses nodes in k-sized groups in a linked list.
        If the number of remaining nodes is less than k, they are left as-is.

        This is a **recursive implementation**.

        :param head: The head of the linked list.
        :param k: The group size for reversal.
        :return: The head of the modified linked list.
        """
        
        cur = head
        group = 0
        
        # Step 1: Check if there are at least k nodes in the current segment
        while cur and group < k:
            cur = cur.next  # Move pointer ahead
            group += 1  # Count the nodes
        
        if group == k:  # If we have k nodes, we proceed with reversal
            cur = self.reverseKGroup(cur, k)  # Recursive call for the next k-group
            
            # Step 2: Reverse the current k-group
            while group > 0:
                tmp = head.next  # Store next node
                head.next = cur  # Point current node to the reversed part
                cur = head  # Move cur pointer ahead
                head = tmp  # Move head pointer ahead
                group -= 1  # Reduce group count
            
            head = cur  # Update the new head after reversal
        
        return head  # Return new head of the linked list
```

### APPROACH: ITERATION

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Import Optional for type hinting

class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        """
        Reverses nodes in k-sized groups in a linked list.
        If the number of remaining nodes is less than k, they remain as is.

        This implementation uses an **iterative approach** with O(1) space.

        :param head: The head of the linked list.
        :param k: The group size for reversal.
        :return: The head of the modified linked list.
        """
        
        dummy = ListNode(0, head)  # Dummy node simplifies edge cases
        groupPrev = dummy  # Pointer to track the previous group

        while True:
            # Step 1: Find the k-th node from groupPrev
            kth = self.getKth(groupPrev, k)
            if not kth:  # If fewer than k nodes remain, exit
                break
            groupNext = kth.next  # Store the node after the k-group

            # Step 2: Reverse the current k-group
            prev, curr = kth.next, groupPrev.next  # Reverse from groupPrev.next to kth
            while curr != groupNext:
                tmp = curr.next  # Store the next node
                curr.next = prev  # Reverse the link
                prev = curr  # Move prev forward
                curr = tmp  # Move curr forward

            # Step 3: Connect reversed group with previous group
            tmp = groupPrev.next  # Store the start of the reversed group
            groupPrev.next = kth  # Connect previous group to new head (kth)
            groupPrev = tmp  # Move groupPrev to the end of the reversed group

        return dummy.next  # Return the new head, skipping dummy

    def getKth(self, curr: Optional[ListNode], k: int) -> Optional[ListNode]:
        """
        Returns the k-th node from the given starting node.
        If there are fewer than k nodes, returns None.

        :param curr: The starting node.
        :param k: The number of nodes to move forward.
        :return: The k-th node or None if fewer than k nodes remain.
        """
        while curr and k > 0:
            curr = curr.next
            k -= 1
        return curr
```
