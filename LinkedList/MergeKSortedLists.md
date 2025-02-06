# 23. Merge K Sorted Lists
Difficulty: Hard

## QUESTION

You are given an array of `k` linked lists `lists`, where each list is sorted in ascending order.

Return the sorted linked list that is the result of merging all of the individual linked lists.

### EXAMPLE

```
Input: lists = [[1,2,4],[1,3,5],[3,6]]
Output: [1,1,2,3,3,4,5,6]
```

```
Input: lists = []
Output: []
```

```
Input: lists = [[]]
Output: []
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import List, Optional  # Importing List and Optional for type hinting

class Solution:    
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        """
        Merges k sorted linked lists into one sorted linked list.

        This implementation:
        - Extracts all values from the linked lists into a list.
        - Sorts the extracted values.
        - Reconstructs a sorted linked list from the sorted values.

        :param lists: A list containing k sorted linked lists.
        :return: The head of the merged sorted linked list.
        """

        nodes = []  # List to store all values from the linked lists

        # Step 1: Extract values from all linked lists
        for lst in lists:
            while lst:  # Traverse each linked list
                nodes.append(lst.val)  # Store node values
                lst = lst.next  # Move to the next node

        # Step 2: Sort the extracted values
        nodes.sort()  # Sorting takes O(N log N), where N is the total number of nodes

        # Step 3: Reconstruct the sorted linked list
        res = ListNode(0)  # Dummy node to simplify handling of the head
        cur = res  # Pointer to build the new linked list

        for node in nodes:
            cur.next = ListNode(node)  # Create a new node and link it
            cur = cur.next  # Move to the next node

        return res.next  # Return the merged linked list, skipping the dummy node
```

### APPROACH: MERGE LISTS

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import List, Optional  # Importing List and Optional for type hinting

class Solution:    
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        """
        Merges k sorted linked lists into one sorted linked list.

        This implementation uses an iterative **pairwise merging approach**.

        :param lists: A list containing k sorted linked lists.
        :return: The head of the merged sorted linked list.
        """
        
        if len(lists) == 0:  # Edge case: If there are no linked lists, return None
            return None

        # Iteratively merge lists one by one
        for i in range(1, len(lists)):  # Start merging from the second list
            lists[i] = self.mergeList(lists[i - 1], lists[i])  # Merge previous result with current list
        
        return lists[-1]  # The last list contains the fully merged result

    def mergeList(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        """
        Merges two sorted linked lists into one sorted list (like in Merge Sort).

        :param l1: First sorted linked list.
        :param l2: Second sorted linked list.
        :return: The head of the merged sorted linked list.
        """
        
        dummy = ListNode()  # Dummy node to simplify handling of the head
        tail = dummy  # Pointer to build the merged list

        # Merge lists by choosing the smaller node at each step
        while l1 and l2:
            if l1.val < l2.val:  # If l1's value is smaller, attach it
                tail.next = l1
                l1 = l1.next  # Move to the next node in l1
            else:  # If l2's value is smaller or equal, attach it
                tail.next = l2
                l2 = l2.next  # Move to the next node in l2
            tail = tail.next  # Move the tail forward
        
        # Attach the remaining nodes from l1 or l2
        if l1:
            tail.next = l1
        if l2:
            tail.next = l2
        
        return dummy.next  # Return the merged list (skipping dummy node)
```

### APPROACH: DIVIDE AND CONQUER

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val  # Stores the value of the node
#         self.next = next  # Pointer to the next node in the linked list

from typing import List, Optional  # Importing List and Optional for type hinting

class Solution:
    
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        """
        Merges k sorted linked lists into one sorted linked list.

        This implementation uses a **Divide & Conquer approach**:
        - Recursively merges pairs of lists.
        - Reduces the number of lists to merge at each step.
        - Achieves O(N log k) time complexity.

        :param lists: A list containing k sorted linked lists.
        :return: The head of the merged sorted linked list.
        """

        if not lists or len(lists) == 0:  # Edge case: If no lists exist, return None
            return None

        # Keep merging lists in pairs until only one list remains
        while len(lists) > 1:
            mergedLists = []  # Temporary list to store merged lists
            
            # Iterate through lists in pairs (i, i+1)
            for i in range(0, len(lists), 2):
                l1 = lists[i]  # First list
                l2 = lists[i + 1] if (i + 1) < len(lists) else None  # Second list (if exists)
                mergedLists.append(self.mergeList(l1, l2))  # Merge and add to the list
            
            lists = mergedLists  # Update the lists with newly merged lists
        
        return lists[0]  # The last remaining list is the fully merged list

    def mergeList(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        """
        Merges two sorted linked lists into one sorted list (like in Merge Sort).

        :param l1: First sorted linked list.
        :param l2: Second sorted linked list.
        :return: The head of the merged sorted linked list.
        """
        
        dummy = ListNode()  # Dummy node to simplify handling of the head
        tail = dummy  # Pointer to build the merged list

        # Merge lists by choosing the smaller node at each step
        while l1 and l2:
            if l1.val < l2.val:  # If l1's value is smaller, attach it
                tail.next = l1
                l1 = l1.next  # Move to the next node in l1
            else:  # If l2's value is smaller or equal, attach it
                tail.next = l2
                l2 = l2.next  # Move to the next node in l2
            tail = tail.next  # Move the tail forward
        
        # Attach the remaining nodes from l1 or l2
        if l1:
            tail.next = l1
        if l2:
            tail.next = l2
        
        return dummy.next  # Return the merged list (skipping dummy node)
```
