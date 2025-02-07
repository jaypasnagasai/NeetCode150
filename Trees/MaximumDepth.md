# 104. Maximum Depth of Binary Tree 
Difficulty: Easy

## QUESTION

Given the `root` of a binary tree, return its depth.

The depth of a binary tree is defined as the number of nodes along the longest path from the root node down to the farthest leaf node.

### EXAMPLE

```
Input: root = [1,2,3,null,null,4]
Output: 3
```

```
Input: root = []
Output: 0
```

## SOLUTION


### APPROACH: BREADTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Stores the value of the node
#         self.left = left  # Pointer to the left child
#         self.right = right  # Pointer to the right child

from typing import Optional  # Import Optional for type hinting

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        """
        Finds the maximum depth (height) of a binary tree using **iterative DFS** (stack-based approach).

        :param root: The root of the binary tree.
        :return: The maximum depth of the tree.
        """
        
        stack = [[root, 1]]  # Stack stores [node, current_depth]
        res = 0  # Variable to track the maximum depth

        while stack:
            node, depth = stack.pop()  # Pop the last inserted node (DFS behavior)

            if node:
                res = max(res, depth)  # Update max depth found so far
                stack.append([node.left, depth + 1])  # Push left child with updated depth
                stack.append([node.right, depth + 1])  # Push right child with updated depth
        
        return res  # Return the maximum depth recorded
```

### APPROACH: DEPTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Stores the value of the node
#         self.left = left  # Pointer to the left child
#         self.right = right  # Pointer to the right child

from typing import Optional  # Import Optional for type hinting
from collections import deque  # Import deque for efficient queue operations

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        """
        Finds the maximum depth (height) of a binary tree using **BFS (level-order traversal)**.

        :param root: The root of the binary tree.
        :return: The maximum depth of the tree.
        """
        
        q = deque()  # Initialize a queue for BFS traversal
        
        if root:
            q.append(root)  # Add the root node to the queue if it exists

        level = 0  # Variable to track the number of levels (depth)

        while q:  # Loop until the queue is empty
            for i in range(len(q)):  # Process all nodes at the current level
                node = q.popleft()  # Remove the node from the front of the queue
                
                # Enqueue left and right children (if they exist)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)

            level += 1  # Increment depth after processing a level
        
        return level  # Return the total depth of the tree
```

