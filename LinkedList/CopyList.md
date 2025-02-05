# 138. Copy List With Random Pointer
Difficulty: Medium

## QUESTION

You are given the head of a linked list of length `n`. Unlike a singly linked list, each node contains an additional pointer `random`, which may point to any node in the list, or `null`.

Create a deep copy of the list.

The deep copy should consist of exactly `n` new nodes, each including:

- The original value `val` of the copied node
- A `next` pointer to the new node corresponding to the `next` pointer of the original node
- A `random` pointer to the new node corresponding to the `random` pointer of the original node

Note: None of the pointers in the new list should point to nodes in the original list.

Return the head of the copied linked list.

In the examples, the linked list is represented as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where `random_index` is the index of the node (0-indexed) that the `random` pointer points to, or `null` if it does not point to any node.

### EXAMPLE

```
Input: head = [[3,null],[7,3],[4,0],[5,1]]
Output: [[3,null],[7,3],[4,0],[5,1]]
```

```
Input: head = [[1,null],[2,2],[3,2]]
Output: [[1,null],[2,2],[3,2]]
```

## SOLUTION


### APPROACH: HASH MAP

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)  # Stores the value of the node
        self.next = next  # Pointer to the next node in the linked list
        self.random = random  # Pointer to any random node in the list (or None)
"""

from typing import Optional

class Solution:
    def __init__(self):
        """
        Initializes a dictionary (self.map) to store mappings from original nodes
        to their corresponding copied nodes to prevent duplicate copies.
        """
        self.map = {}

    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        """
        Creates a deep copy of a linked list where each node contains an additional 
        random pointer that could point to any node in the list (or None).

        This implementation uses recursion with memoization to avoid duplicate copies.

        :param head: The head node of the original linked list.
        :return: The head node of the copied linked list.
        """
        # Base case: If head is None, return None (end of the list)
        if head is None:
            return None

        # If this node was already copied, return the stored copy to prevent duplication
        if head in self.map:
            return self.map[head]
        
        # Step 1: Create a new node with the same value as the original node
        copy = Node(head.val)

        # Store the copy in the dictionary before making recursive calls
        # to prevent infinite loops in cyclic linked lists
        self.map[head] = copy

        # Step 2: Recursively copy the next node
        copy.next = self.copyRandomList(head.next)

        # Step 3: Assign the copied random pointer (use dictionary to retrieve the correct copy)
        copy.random = self.copyRandomList(head.random)

        # Step 4: Return the newly created copy node
        return copy
```

### APPROACH: SPACE OPTIMIZED - I

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)  # Stores the value of the node
        self.next = next  # Pointer to the next node in the linked list
        self.random = random  # Pointer to any random node in the list (or None)
"""

from typing import Optional

class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        """
        Creates a deep copy of a linked list where each node contains an additional 
        random pointer that could point to any node in the list (or None).

        This implementation follows an **iterative three-step approach** using **O(1) extra space**.

        :param head: The head node of the original linked list.
        :return: The head node of the copied linked list.
        """
        
        if head is None:
            return None  # If the original list is empty, return None
        
        # Step 1: Interweaving cloned nodes into the original list
        l1 = head
        while l1 is not None:
            l2 = Node(l1.val)  # Create a new node (copy)
            l2.next = l1.next  # Connect the new node's next to l1's next
            l1.next = l2  # Insert the new node after l1
            l1 = l2.next  # Move to the next original node
            
        # Step 2: Assign the correct random pointers to copied nodes
        l1 = head
        while l1 is not None:
            if l1.random is not None:  # If the original node has a random pointer
                l1.next.random = l1.random.next  # Set the cloned node's random pointer
            l1 = l1.next.next  # Move to the next original node
        
        # Step 3: Separate the copied nodes to form the deep copy list
        newHead = head.next  # Store the head of the copied list
        l1 = head
        while l1 is not None:
            l2 = l1.next  # Copied node
            l1.next = l2.next  # Restore the original node's next pointer
            if l2.next is not None:  # If there's another copied node
                l2.next = l2.next.next  # Connect copied node's next pointer to the next copied node
            l1 = l1.next  # Move to the next original node
            
        return newHead  # Return the head of the deep copied linked list
```

### APPROACH: SPACE OPTIMIZED - II

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)  # Stores the value of the node
        self.next = next  # Pointer to the next node in the linked list
        self.random = random  # Pointer to any random node in the list (or None)
"""

from typing import Optional

class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        """
        Creates a deep copy of a linked list where each node contains an additional 
        random pointer that could point to any node in the list (or None).

        This implementation follows a three-pass approach using **O(1) extra space**.

        :param head: The head node of the original linked list.
        :return: The head node of the copied linked list.
        """
        
        if head is None:
            return None  # If the original list is empty, return None
        
        # Step 1: Create new copy nodes and insert them into the original list
        l1 = head
        while l1:
            l2 = Node(l1.val)  # Create a new node (copy)
            l2.next = l1.random  # Temporarily store original random pointer in l2.next
            l1.random = l2  # Link original node's random pointer to its copy
            l1 = l1.next  # Move to the next original node
        
        # Step 2: Set the correct random pointers for the copied nodes
        newHead = head.random  # The copied head node
        l1 = head
        while l1:
            l2 = l1.random  # Retrieve the copied node
            l2.random = l2.next.random if l2.next else None  # Assign correct random pointer
            l1 = l1.next  # Move to the next original node
            
        # Step 3: Restore the original list and set the correct next pointers for the copied list
        l1 = head
        while l1:
            l2 = l1.random  # Retrieve the copied node
            l1.random = l2.next  # Restore original node's random pointer
            l2.next = l1.next.random if l1.next else None  # Set copied node's next pointer
            l1 = l1.next  # Move to the next original node

        return newHead  # Return the head of the copied linked list
```
