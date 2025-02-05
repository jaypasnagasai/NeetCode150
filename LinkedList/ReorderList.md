# 143. Reorder List
Difficulty: Medium

## QUESTION

You are given the head of a singly linked-list.

The positions of a linked list of `length = 7` for example, can intially be represented as:

`[0, 1, 2, 3, 4, 5, 6]`

Reorder the nodes of the linked list to be in the following order:

`[0, 6, 1, 5, 2, 4, 3]`

Notice that in the general case for a list of `length = n` the nodes are reordered to be in the following order:

`[0, n-1, 1, n-2, 2, n-3, ...]`

You may not modify the values in the list's nodes, but instead you must reorder the nodes themselves.

### EXAMPLE

```
Input: head = [2,4,6,8]
Output: [2,8,4,6]
```

```
Input: head = [2,4,6,8,10]
Output: [2,10,4,8,6]
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
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Reorders a linked list in a specific pattern:
        L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...

        Modifies the linked list in-place without returning anything.

        :param head: The head of the linked list.
        """
        if not head:  # Edge case: If the list is empty, return immediately
            return
        
        # Step 1: Store all nodes in an array
        nodes = []
        cur = head
        while cur:
            nodes.append(cur)  # Append each node to the list
            cur = cur.next  # Move to the next node
        
        # Step 2: Reorder nodes by modifying the next pointers
        i, j = 0, len(nodes) - 1  # Two-pointer approach: i (start), j (end)
        while i < j:
            nodes[i].next = nodes[j]  # Connect current start node to the last node
            i += 1  # Move start pointer forward
            if i >= j:  # Check if the pointers have crossed
                break
            nodes[j].next = nodes[i]  # Connect current end node to the next start node
            j -= 1  # Move end pointer backward
        
        # Step 3: Ensure the last node points to None to prevent cycles
        nodes[i].next = None
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
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Reorders the linked list in a specific pattern:
        L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...

        This implementation uses recursion to achieve the reordering.
        It modifies the linked list in-place and does not return anything.

        :param head: The head of the linked list.
        """

        def rec(root: ListNode, cur: ListNode) -> ListNode:
            """
            Recursive function to reorder the list.

            :param root: The pointer to the start of the list (moves forward in recursion).
            :param cur: The pointer traversing towards the end of the list (backtracking).
            :return: The updated root pointer for the next step in reordering.
            """
            if not cur:  # Base case: If cur is None, return root (backtrack from the end)
                return root
            
            # Recursive call to reach the end of the list first
            root = rec(root, cur.next)

            if not root:  # If root is None, stop processing
                return None
            
            tmp = None  # Temporary pointer for swapping
            
            # If root and cur are the same or adjacent, we reach the middle of the list
            if root == cur or root.next == cur:
                cur.next = None  # Ensure the last node points to None to terminate properly
            else:
                tmp = root.next  # Store reference to the next node in the reordered sequence
                root.next = cur  # Link root (first node) to cur (last node)
                cur.next = tmp  # Link last node to the next element in the reordered list
            
            return tmp  # Return next node to process in reordering
        
        head = rec(head, head.next)  # Start recursive process from the second node
```

### APPROACH: REVERSE AND MERGE

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import Optional  # Importing Optional for type hinting

class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Reorders the linked list in a specific pattern:
        L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...

        This implementation follows a three-step approach:
        1. Find the middle of the linked list using slow and fast pointers.
        2. Reverse the second half of the list.
        3. Merge the two halves to get the desired order.

        :param head: The head of the linked list.
        """

        # Step 1: Find the middle of the linked list
        slow, fast = head, head.next  # 'slow' moves 1 step, 'fast' moves 2 steps
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        # Step 2: Reverse the second half of the linked list
        second = slow.next  # Start of the second half
        prev = slow.next = None  # Disconnect first half from the second half
        while second:  # Reverse the second half
            tmp = second.next
            second.next = prev
            prev = second
            second = tmp

        # Step 3: Merge the first and reversed second half
        first, second = head, prev  # 'first' is start of first half, 'second' is start of reversed second half
        while second:  # Interleave nodes
            tmp1, tmp2 = first.next, second.next  # Store next pointers
            first.next = second  # Connect first node to second
            second.next = tmp1  # Connect second node to the next node in first half
            first, second = tmp1, tmp2  # Move both pointers forward
```
