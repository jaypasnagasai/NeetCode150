# 226. Invert Binary Trees
Difficulty: Easy

## QUESTION

You are given the root of a binary tree `root`. Invert the binary tree and return its root.

### EXAMPLE

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,3,2,7,6,5,4]
```

```
Input: root = [3,2,1]
Output: [3,1,2]
```

```
Input: root = []
Output: []
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
from collections import deque  # Import deque for efficient queue operations

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        """
        Inverts a binary tree by swapping left and right children at each level.
        
        This implementation uses **BFS (level-order traversal)** with a queue.

        :param root: The root of the binary tree.
        :return: The root of the inverted binary tree.
        """

        if not root:  # If the tree is empty, return None
            return None
        
        queue = deque([root])  # Initialize queue with the root node
        
        while queue:
            node = queue.popleft()  # Process the current node
            
            # Swap left and right child nodes
            node.left, node.right = node.right, node.left  
            
            # Add children to queue if they exist
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        return root  # Return the root of the inverted tree
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

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        """
        Inverts a binary tree by recursively swapping left and right children.

        This implementation uses **Depth-First Search (DFS - Preorder Traversal)**.

        :param root: The root of the binary tree.
        :return: The root of the inverted binary tree.
        """
        
        if not root:  # Base case: If tree is empty, return None
            return None

        # Swap left and right child nodes
        root.left, root.right = root.right, root.left
        
        # Recursively invert the left and right subtrees
        self.invertTree(root.left)
        self.invertTree(root.right)
        
        return root  # Return the root of the inverted tree
```
