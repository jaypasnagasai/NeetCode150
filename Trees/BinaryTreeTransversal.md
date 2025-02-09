# 102. Binary Tree Level Order Traversal
Difficulty: Medium

## QUESTION

Given a binary tree `root`, return the level order traversal of it as a nested list, where each sublist contains the values of nodes at a particular level in the tree, from left to right.

### EXAMPLE

```
Input: root = [1,2,3,4,5,6,7]
Output: [[1],[2,3],[4,5,6,7]]
```

```
Input: root = [1]
Output: [[1]]
```

```
Input: root = []
Output: []
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
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        """
        Performs level order traversal (BFS) on a binary tree and returns 
        a list of node values for each level.

        :param root: Root node of the binary tree
        :return: A list of lists, where each inner list contains the values of nodes at that level
        """

        res = []  # List to store the node values level-wise

        def dfs(node, depth):
            """
            Uses Depth-First Search (DFS) to traverse the tree and store node values by level.

            :param node: Current node in the traversal
            :param depth: Current depth (or level) in the tree
            """

            if not node:  # Base case: if node is None, return
                return None

            # If the current depth is not yet in the result list, add a new sublist
            if len(res) == depth:
                res.append([])

            # Append the current node value to its corresponding depth level
            res[depth].append(node.val)

            # Recur for the left and right child nodes, increasing the depth
            dfs(node.left, depth + 1)
            dfs(node.right, depth + 1)
        
        # Start DFS traversal from the root at depth 0
        dfs(root, 0)
        
        return res  # Return the list of level-wise node values
```

### APPROACH: BREADTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

import collections  # Import collections module for deque

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        """
        Performs level-order traversal (BFS) on a binary tree and returns 
        a list of node values for each level.

        :param root: Root node of the binary tree
        :return: A list of lists, where each inner list contains the values of nodes at that level
        """

        res = []  # List to store the node values level-wise

        # Use a queue to facilitate BFS traversal
        q = collections.deque()
        q.append(root)  # Start with the root node

        while q:
            qLen = len(q)  # Get the number of nodes at the current level
            level = []  # List to store values of the current level

            # Process all nodes at the current level
            for i in range(qLen):
                node = q.popleft()  # Dequeue the front node

                if node:  # If the node is not None, process it
                    level.append(node.val)  # Add node value to the level list
                    q.append(node.left)  # Enqueue left child
                    q.append(node.right)  # Enqueue right child
            
            if level:  # Append level to result if it's not empty
                res.append(level)
                
        return res  # Return the list of level-wise node values
```
