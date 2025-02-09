# 199. Binary Tree Right Side View
Difficulty: Medium

## QUESTION

You are given the `root` of a binary tree. Return only the values of the nodes that are visible from the right side of the tree, ordered from top to bottom.

### EXAMPLE

```
Input: root = [1,2,3]
Output: [1,3]
```

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,3,7]
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
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        """
        Returns the right-side view of a binary tree.
        The right-side view consists of the nodes visible when looking at the tree from the right.

        :param root: Root node of the binary tree
        :return: A list of node values representing the right-side view
        """

        res = []  # List to store right-side view node values

        def dfs(node, depth):
            """
            Performs a depth-first search (DFS) to capture the rightmost node at each level.

            :param node: Current node in the traversal
            :param depth: Current depth (or level) in the tree
            """
            if not node:  # Base case: if the node is None, return
                return None

            # If we are visiting this depth for the first time, store the rightmost node value
            if depth == len(res):
                res.append(node.val)

            # Prioritize right subtree first, ensuring the rightmost nodes are stored first
            dfs(node.right, depth + 1)
            dfs(node.left, depth + 1)

        # Start DFS traversal from the root at depth 0
        dfs(root, 0)

        return res  # Return the list of right-side view node values
```

### APPROACH: BREADTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

from collections import deque  # Import deque for efficient queue operations

class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        """
        Returns the right-side view of a binary tree.
        The right-side view consists of the nodes visible when looking at the tree from the right.

        :param root: Root node of the binary tree
        :return: A list of node values representing the right-side view
        """

        res = []  # List to store right-side view node values
        q = deque([root])  # Initialize a queue with the root node

        while q:  # Continue traversal while there are nodes in the queue
            rightSide = None  # Variable to track the rightmost node at the current level
            qLen = len(q)  # Number of nodes at the current level

            # Process all nodes at the current level
            for i in range(qLen):
                node = q.popleft()  # Dequeue the front node

                if node:  # If node is not None, process it
                    rightSide = node  # Update rightSide to the last node at this level
                    q.append(node.left)  # Enqueue left child
                    q.append(node.right)  # Enqueue right child

            if rightSide:  # If rightmost node is found, add its value to result
                res.append(rightSide.val)

        return res  # Return the list of right-side view node values
```
