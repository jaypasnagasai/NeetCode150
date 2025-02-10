# 1448. Count Good Nodes In Binary Tree 
Difficulty: Medium

## QUESTION

Within a binary tree, a node `x` is considered good if the path from the root of the tree to the node `x` contains no nodes with a value greater than the value of node `x`

Given the `root` of a binary tree root, return the number of good nodes within the tree.

### EXAMPLE

```
Input: root = [2,1,1,3,null,1,5]
Output: 3
```

```
Input: root = [1,2,-1,3,4]
Output: 4
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
    def goodNodes(self, root: TreeNode) -> int:
        """
        Counts the number of "good" nodes in a binary tree.
        A node is considered "good" if its value is greater than or equal to all the node values along the path from the root to that node.

        :param root: Root node of the binary tree
        :return: The number of good nodes in the tree
        """

        def dfs(node, maxVal):
            """
            Performs Depth-First Search (DFS) to count good nodes.

            :param node: Current node being processed
            :param maxVal: Maximum value encountered along the path to this node
            :return: Count of good nodes in the subtree rooted at this node
            """
            if not node:  # Base case: if the node is None, return 0
                return 0

            # A node is good if its value is greater than or equal to maxVal seen so far
            res = 1 if node.val >= maxVal else 0

            # Update maxVal to be the maximum of the current node value and maxVal
            maxVal = max(maxVal, node.val)

            # Recursively check left and right subtrees
            res += dfs(node.left, maxVal)
            res += dfs(node.right, maxVal)

            return res  # Return total count of good nodes in this subtree

        # Start DFS traversal from the root with its own value as the initial maxVal
        return dfs(root, root.val)
```

### APPROACH: BREADTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

from collections import deque  # Import deque for efficient queue-based BFS

class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        """
        Counts the number of "good" nodes in a binary tree using Breadth-First Search (BFS).
        A node is considered "good" if its value is greater than or equal to all the node values along the path from the root to that node.

        :param root: Root node of the binary tree
        :return: The number of good nodes in the tree
        """

        res = 0  # Counter for good nodes
        q = deque()  # Initialize a queue for BFS traversal

        # Start BFS with the root node and its value as the initial maxVal
        q.append((root, -float('inf')))  # (-inf ensures root is always counted as good)

        while q:  # Continue traversal while there are nodes in the queue
            node, maxval = q.popleft()  # Dequeue a node and its maxVal

            # If the current node's value is greater than or equal to maxVal seen so far, it's a good node
            if node.val >= maxval:  
                res += 1  # Increment good node count

            # Update maxVal for the next level of traversal
            new_max = max(maxval, node.val)

            # Enqueue left and right children, along with the updated maxVal
            if node.left:    
                q.append((node.left, new_max))
            
            if node.right:
                q.append((node.right, new_max))
                
        return res  # Return the total count of good nodes
```

