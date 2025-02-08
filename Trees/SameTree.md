# 100. Same Tree
Difficulty: Easy

## QUESTION

Given the roots of two binary trees `p` and `q`, return `true` if the trees are equivalent, otherwise return `false`.

Two binary trees are considered equivalent if they share the exact same structure and the nodes have the same values.

### EXAMPLE

```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

```
Input: p = [4,7], q = [4,null,7]
Output: false
```

```
Input: p = [1,2,3], q = [1,3,2]
Output: false
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        """
        Determines if two binary trees are structurally identical and have the same node values.
        
        :param p: Root node of the first binary tree
        :param q: Root node of the second binary tree
        :return: True if both trees are the same, False otherwise
        """

        stack = [(p, q)]  # Stack to store node pairs for iterative comparison

        while stack:
            node1, node2 = stack.pop()  # Pop a pair of nodes to compare

            # If both nodes are None, continue to the next pair (they match)
            if not node1 and not node2:
                continue

            # If only one of the nodes is None, or their values do not match, return False
            if not node1 or not node2 or node1.val != node2.val:
                return False

            # Push corresponding right children onto the stack for comparison
            stack.append((node1.right, node2.right))

            # Push corresponding left children onto the stack for comparison
            stack.append((node1.left, node2.left))

        # If all nodes match, return True
        return True
```

### APPROACH: BREADTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

from collections import deque  # Importing deque for efficient queue operations

class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        """
        Determines if two binary trees are structurally identical and have the same node values,
        using a level-order (breadth-first) traversal.

        :param p: Root node of the first binary tree
        :param q: Root node of the second binary tree
        :return: True if both trees are the same, False otherwise
        """

        # Initialize two queues for level-order traversal of both trees
        q1 = deque([p])
        q2 = deque([q])

        while q1 and q2:  # Continue until one or both queues are empty
            for _ in range(len(q1)):  # Process nodes at the current level
                nodeP = q1.popleft()  # Dequeue a node from tree p
                nodeQ = q2.popleft()  # Dequeue a node from tree q

                # If both nodes are None, continue to the next pair (they match)
                if nodeP is None and nodeQ is None:
                    continue

                # If only one of the nodes is None, or their values do not match, return False
                if nodeP is None or nodeQ is None or nodeP.val != nodeQ.val:
                    return False

                # Enqueue the left and right children of both nodes for further comparison
                q1.append(nodeP.left)
                q1.append(nodeP.right)
                q2.append(nodeQ.left)
                q2.append(nodeQ.right)

        # If all nodes match, return True
        return True
```
