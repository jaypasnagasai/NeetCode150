# 21. Merge Two Sorted Lists
Difficulty: Easy

## QUESTION

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one sorted linked list and return the head of the new sorted linked list.

The new list should be made up of nodes from `list1` and `list2`.

### EXAMPLE

```
Input: list1 = [1,2,4], list2 = [1,3,5]
Output: [1,1,2,3,4,5]
```

```
Input: list1 = [], list2 = [1,2]
Output: [1,2]
```

```
Input: list1 = [], list2 = []
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
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        # Base cases: if either list is empty, return the other list
        if list1 is None:
            return list2
        if list2 is None:
            return list1

        # Recursively merge the lists based on the smaller value
        if list1.val <= list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)  # Continue merging with list1.next
            return list1  # Return list1 as the head of the merged list
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)  # Continue merging with list2.next
            return list2  # Return list2 as the head of the merged list
```

### APPROACH: ITERATION

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def mergeTwoLists(self, list1: ListNode, list2: ListNode) -> ListNode:
        # Create a dummy node to act as the starting point of the merged list
        dummy = node = ListNode()

        # Traverse both lists while they are not empty
        while list1 and list2:
            if list1.val < list2.val:  # Choose the smaller node
                node.next = list1  # Link the current node to list1
                list1 = list1.next  # Move list1 to the next node
            else:
                node.next = list2  # Link the current node to list2
                list2 = list2.next  # Move list2 to the next node
            node = node.next  # Move the pointer forward in the merged list

        # Attach any remaining nodes from either list1 or list2
        node.next = list1 or list2

        # Return the merged list, starting from the node after the dummy
        return dummy.next
```

